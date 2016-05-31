# ETS2Sync Helper
[ENGLISH VERSION](README-en.md)

Este programa faz a sincronização de cargas no Euro Truck Simulator 2, para facilitar a organização de comboios no Multiplayer.

## Download
http://files.dsantosdev.com/ets2sync_helper.zip

## Como Sincronizar
1. No jogo, crie um save (vá em “Salvar e Carregar” → “Salvar Jogo”, digite um nome, clique em “Salvar”).
2. Pressione Alt+Tab para minimizar o jogo.
3. Abra o programa (ou clique em “Atualizar” se o programa já estava aberto).
4. Se aparecer alguma mensagem em vermelho:
	* “Formato do Save incorreto”:
		1. Saia do jogo. Esta etapa não funcionará se o jogo estiver em execução.
		2. No programa, clique no botão “Corrigir”.
		3. Confirme as mensagens.
		4. Entre no jogo novamente.
		5. Volte para o passo 1.
	* “Nenhum save encontrado”/“*x* saves, nenhum é compatível” (*x* sendo um número qualquer):
		1. Clique em “Atualizar”.
		2. Certifique-se de que o perfil correto está selecionado.
		3. Volte para o passo 1.
5. Se não há nenhuma mensagem em vermelho, clique em “Sincronizar”.
6. Aguarde até que apareça a mensagem “Sincronização concluída”.
7. Volte para o jogo. Carregue o save que foi criado no passo 1 (vá em “Salvar e Carregar” → “Carregar Jogo”).
8. No “Mercado de Fretes”, verifique se nas cargas o campo “A oferta expira em” é o mesmo para todas (e que é cerca de 500 horas). Se for, a sincronização foi feita com sucesso.

	Se as cargas não estão todas com o mesmo tempo para expirar, isso significa que a sincronização falhou. Normalmente isso acontece porque a pessoa esqueceu de carregar o save depois de sincronizar, ou que o save que foi carregado não é o que foi sincronizado (confira o perfil e o save selecionados no programa).

## Bugs/Problemas
Se você encontrar um problema no programa, informe no link abaixo:

https://github.com/davidsantos-br/ets2sync_helper/issues

## Desenvolvimento
**Nota:** Esta etapa não é necessária para sincronizar as cargas.

Se você deseja executar o programa a partir do código-fonte e/ou colaborar com o desenvolvimento:
1. Clone o repositório.
2. Instale o Ruby 2.2.x (http://rubyinstaller.org/).
3. Instale a gem `qtbindings`:
	```
	gem install qtbindings
	```
4. Se você deseja empacotar um executável, instale a gem `ocra`:
	```
	gem install ocra
	```
5. Execute `verbose.bat` ou `no_console.bat` para executar o programa.
6. Para gerar um executável, execute `build_exe.bat`.

### Bug na gem `qtbindings`

Um bug na gem `qtbindings` relacionado a encodings faz com que o programa não funcione a partir do executável criado pelo `ocra` se o nome de usuário no Windows da pessoa executando o programa tiver acentos ou outros caracteres especiais. Eu constatei que as seguintes alterações corrigem o problema:

#### Gem `qtbindings`
No arquivo `lib/Qt4.rb`, substituir as linhas 12-17 pelas linhas abaixo:
```ruby
ruby_version = RUBY_VERSION.split('.')[0..1].join('.').encode("UTF-8")
if windows
	ENV['PATH'] = (File.join(File.dirname(__FILE__).encode("UTF-8"), '../bin') + ';' + File.join(File.dirname(__FILE__).encode("UTF-8"), "../lib/#{ruby_version}") + ';' + File.join(File.dirname(__FILE__).encode("UTF-8"), "../bin/#{ruby_version}") + ';' + ENV['PATH'].encode("UTF-8")).encode(ENV['PATH'].encoding)
end
$: << File.join(File.dirname(__FILE__).encode("UTF-8"), "../lib/#{ruby_version}").encode("filesystem")
require "#{ruby_version}/qtruby4"
```

#### Gem `qtbindings-qt` (dependência de `qtbindings`):
No arquivo `qtlib/qtbindings-qt.rb`, substituir a linha 8 pela linha abaixo:
```ruby
ENV['PATH'] = (File.join(File.dirname(__FILE__).encode("UTF-8"), '../qtbin') + ';' + File.join(File.dirname(__FILE__).encode("UTF-8"), '../qtbin/plugins') + ';' + ENV['PATH'].encode("UTF-8")).encode(ENV['PATH'].encoding)
```