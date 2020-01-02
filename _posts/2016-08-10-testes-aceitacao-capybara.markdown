---
layout: post
title: Testes de Aceitação com Capybara/Rspec e Page Objects
date: 2016-08-10 00:00:00 +0300
description: Um pouco sobre como arquitetar seus testes com Capybara e Page Objects # Add post description (optional)
img: rspec.png # Add image post (optional)
fig-caption: rpsec # Add figcaption (optional)
tags: [Portuguese, Ruby, Rspec, Capybara, Page Objects] # add tag
---

Já abordei um pouco sobre testes unitários com mocks. Particularmente não tenho nenhuma preferência em fazer testes da forma mockista ou da forma classicista, creio que as duas formas são interessantes e servem à propósitos distintos. Enquanto uma está preocupada com a forma como os objetos colaboram entre si, a outra está preocupada no resultado final. Ainda com relação a quantidade de colaboradores com as quais os objetos se preocupam, podemos chama-los de testes unitários, de integração, de aceitação e muitos outros nomes.

Em programação orientada à objetos, um teste unitário se preocupa com apenas um objeto, ele testa apenas o comportamento daquele objeto, no entanto isto não é uma regra para ser levada ao pé da letra. Alguns testes, tidos como unitários, acabam testando um pouco além das fronteiras do próprio objeto ou da própria unidade à ser testada.

Existe uma discussão muito grande na comunidade sobre as vantagens de se realizar testes unitários da forma mockista X a forma classicista. Quando temos um sistema onde muitos objetos colaboram entre si, uma forma de se testar apenas um objeto é mockando ou criando falsamente os objetos colaboradores, fazendo com que o teste só exercite a unidade que de fato deve ser testada. Ao usar os objetos colaboradores reais o teste unitário acabará por testar todos os objetos colaboradores e isso se caracterizará como um teste de integração entre os objetos e não mais um teste puramente unitário. Como disse a discussão é muito grande e não pretendo incita-la aqui.
Mas um sistema não é composto só de testes unitários. É necessário que existam testes com um nível mais alto de abstração. No caso de um sistema web o nível mais alto de abstração é testar a funcionalidade no browser, como se fosse um usuário.

Existem várias ferramentas para automatizar este processo e em ruby/rails uma das mais populares é a gem Capybara que é usada em conjunto com RSpec para gerar testes chamados de aceitação, ou que simulam o caso de uso como um usuário deve usá-lo, de forma automática. O Capybara em conjunto com RSpec facilita e muito a criação de testes automatizados com o browser mas esses testes podem rapidamente se tornar uma bagunça se não adotarmos táticas e metodologias para reaproveitamento de código, aqui é que entra o padrão page object.

### O padrão Page Object é uma excelente forma de se obter reusabilidade nos seus códigos de testes com browser.

Normalmente quando escrevemos testes relacionados à interações com elementos html ou javascript, o código tende a se tornar muito acoplado à página à ser testada. Com Page Objects podemos criar abstrações que acabam por se tornar uma API do caso de uso e escondem a complexidade das APIs para acesso aos campos de um formulário ou tabela por exemplo. Com este padrão podemos também reutilizar o Page Object em outros métodos sempre acessando a sua API, tornando os testes bem mais simples e fáceis de serem lidos. Um outro fator importante é que se mudarmos uma página específica só teremos que mudar em um local nos nossos testes.

## Como Martin Fowler cita em seu famoso texto sobre este padrão:

> The basic rule of thumb for a page object is that it should allow a software client to do anything and see anything that a human can. It should also provide an interface that’s easy to program to and hides the underlying widgetry in the window. So to access a text field you should have accessor methods that take and return a string, check boxes should use booleans, and buttons should be represented by action oriented method names. The page object should encapsulate the mechanics required to find and manipulate the data in the gui control itself. A good rule of thumb is to imagine changing the concrete control — in which case the page object interface shouldn’t change.

O padrão page object, não necessariamente necessita representar uma página, pode representar um elemento complexo de uma página ou um elemento que se repete em várias páginas. Pode ser usado também para testes de desktop em frameworks como Java/Swing e outros. Um Page Object não deve contem nenhum tipo de assert sendo sua única responsabilidade representar uma página ou um objeto de página mais complexo. É bom que seus métodos retornem atributos como string, date, boolean ou um outro page object.

Para demonstrar o uso de page objects eu preferi mostrar um caso CRUD já que é facilmente entendido por todos e é um caso que apesar de simples, consegue demonstrar o poder de reusabilidade e clareza que o padrão page object nos entrega. É claro que você pode optar por fazer testes mais rigorosos do que estes aqui apresentados, mas a intenção aqui é mostrar o uso do padrão e não, ser o exemplo perfeito de testes de software.

### Aqui está o Page Object da página de Login:

```ruby
class LoginPageObject < BaseObject

  def visit_root
    visit '/'
    self
  end

  def login(user)
    fill_in I18n.t('activerecord.attributes.user.email'), with: user.email
    fill_in I18n.t('activerecord.attributes.user.password'), with: user.password
    click_on I18n.t('devise.links.sign_in')
  end
end
```

### Começando com o arquivo de testes do RSpec. Neste arquivo estão os testes de crud:

```ruby
require 'rails_helper'

RSpec.describe 'Residence Integration Tests' do
  let(:user) { create(:user, email: 'admin@admin.com', password: '123456') }
  let(:login_page) { LoginPageObject.new }
  let(:index_page) { IndexResidencePage.new }

  context '#valid_user' do

    before {
      login_page.visit_root.login(user)
    }

    context 'List Residences' do
      let!(:residence) { create(:residence, cep: '57031-530') }


      scenario 'Render the Lists of Residences' do
        index_page.visit_page
        expect(index_page).to have_residence(residence)
      end
    end

    scenario 'Click on new' do
      index_page.visit_page
      new_page = index_page.click_on_new
      expect(new_page).to have_current_path(new_admin_residence_path)
    end

    context "Create a Residence" do
      let(:new_residence) { build(:residence, cep: '57000-000') }

      before {
        index_page.visit_page
      }

      scenario 'Creates a Valid Residence' do
        new_page = index_page.click_on_new
        new_page.fill_in_with_residence(new_residence)
        index_page = new_page.click_on_save
        expect(index_page).to have_message('Domicílio criado com sucesso.')
      end

      scenario 'Create with invalid value' do
        new_page = index_page.click_on_new
        new_page.fill_in_with_residence(new_residence)
        new_page.make_blank_field('#residence_cep')
        new_page.click_on_save
        expect(new_page).to have_invalid_message_on_div('.residence_cep')
      end
    end

    context "Editing a Residence" do
      let!(:residence) { create(:residence, cep: '57031-530') }

      before {
        index_page.visit_page
      }

      scenario 'Edit the residence with success' do
        edit_page = index_page.click_on_edit
        edit_page.change_field('#residence_cep', '57000-000')
        index_page = edit_page.click_on_save
        expect(index_page).to have_message('Domicílio atualizado com sucesso.')
      end

      scenario 'Edit with invalid value' do
        edit_page = index_page.click_on_edit
        edit_page.change_field('#residence_cep', '')
        edit_page.click_on_save
        expect(edit_page).to have_invalid_message_on_div('.residence_cep')
      end
    end

    context "Remove residence" do
      let!(:residence) { create(:residence, cep: '57031-530') }

      scenario 'Remove the residence' do
        index_page.visit_page
        index_page.click_on_remove
        expect(index_page).to have_message('Domicílio foi excluído com sucesso.')
      end
    end
  end
end
```

Este é o Page Object da tela de listagem. Note que o código de mais baixo nível que seria o código de acesso aos elementos de página e código para navegar entre as páginas ou para ir para algum path estão encapsulados em métodos que possuem nomes mais claros. O ideal é testar se todos os elementos que o caso de uso pede estão na tela, mas neste exemplo, só para demonstrar eu verifico, somente, a existência do cep. Note também que os métodos retorna outra página quando há um comportamento que redireciona para uma nova página.

```ruby
class IndexResidencePage < BaseObject

  def visit_page
    visit admin_residences_path
    self
  end

  def has_residence?(residence)
    content = find(:xpath, '//table/tbody/tr')
    has_expected_fields_in_table(content, residence)
  end

  def has_message?(message)
    has_content?(message)
  end

  def click_on_new
    click_link I18n.t('helpers.links.new')
    NewResidencePageObject.new
  end

  def click_on_edit
   click_on I18n.t('helpers.links.edit')
   EditResidencePageObject.new
  end

  def click_on_remove
    click_on I18n.t('helpers.links.destroy')
    self
  end

  private
  def has_expected_fields_in_table(content, residence)
    content.has_content?(residence.name_of_patio) && content.has_content?(residence.cep)
  end

end
```

### A mesma coisa acontece neste Page Object para a tela de novo registro:

```ruby
class NewResidencePageObject < BaseObject

  def has_current_path?(path)
    current_path ==  path
  end

  def fill_in_with_residence(residence)
    find("#residence_latitude").set(residence.latitude)
    find("#residence_longitude").set(residence.longitude)
    fill_in I18n.t('activerecord.attributes.residence.type_of_patio'), with: residence.type_of_patio
    fill_in I18n.t('activerecord.attributes.residence.name_of_patio'), with: residence.name_of_patio
    find("#residence_number").set(residence.number)
    fill_in I18n.t('activerecord.attributes.residence.municipality'), with: residence.municipality
    select('RJ', from: I18n.t('activerecord.attributes.residence.uf'))
    fill_in I18n.t('activerecord.attributes.residence.cep'), with: residence.cep
    fill_in I18n.t('activerecord.attributes.residence.complement'), with: residence.complement
    fill_in I18n.t('activerecord.attributes.residence.residencial_phone'), with: residence.residencial_phone
    choose(I18n.t('activerecord.attributes.residence.housing_situations.own'))
    choose(I18n.t('activerecord.attributes.residence.type_residences.house'))
    fill_in I18n.t('activerecord.attributes.residence.number_of_residents'), with: residence.number_of_residents
    choose(I18n.t('activerecord.attributes.residence.conditions_of_land_uses.owner'))
    choose('residence_has_energy_power_true')
  end

  def make_blank_field(field)
    find(field).set('')
  end

  def click_on_save
    click_on 'Criar Domicílio'
    IndexResidencePage.new
  end

  def has_invalid_message_on_div?(klass)
    within(klass) do
      has_content?(I18n.t('errors.messages.blank'))
    end
  end
end
```



### Aqui está o Page Object de edição:

```ruby
class EditResidencePageObject < BaseObject

  def change_field(id, value)
    find(id).set(value)
  end

  def click_on_save
    click_on 'Atualizar'
    IndexResidencePage.new
  end

  def has_invalid_message_on_div?(klass)
    within(klass) do
      has_content?(I18n.t('errors.messages.blank'))
    end
  end

end
```

Não existe uma regra básica do que colocar em um Page Object a não ser que os métodos de um page object devem retornar strings, boolean, dates e etc ou um outro page object. Algumas pessoas acham que é melhor colocar as assertions dentro dos page objects evitando assim a duplicação de asserts nos testes. Eu, particularmente, acho que um page object deve encapsular o código de mais baixo nível para acesso à uma página ou componente complexo e os asserts devem permanecer onde eles deveriam estar, ou seja, no próprio teste.

Se você curtiu, compartilha e manda para os seus amigos.

## Se quiser me seguir nas redes sociais, você pode seguir através do:

* Youtube: [Youtube Channel(In Portuguese)](https://www.youtube.com/thiagoramosal)
* Twitter: [@thramosal](https://twitter.com/thramosal)
* Instagram: [@thiagoramosal](https://instagram.com/thiagoramosal)

Desejo que você se torne um excelente Dev.
