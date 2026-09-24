# Controle de Abastecimentos e Passageiros

Aplicativo web de página única para registrar abastecimentos e controlar passageiros por TAG da MIP. Funciona diretamente no navegador, sem servidor ou dependências, com navegação lateral entre as duas abas.

## Rodar no computador

Abra `Controle de Abastecimentos Salles Transportes.html` no navegador.

## Publicar no GitHub Pages

1. Crie um repositório no GitHub.
2. Envie `index.html` para a raiz da branch `main` (não apenas o ZIP). Esse arquivo contém a mesma página com o título "Controle de Abastecimentos Salles Transportes".
3. Em **Settings → Pages → Build and deployment**, escolha **Deploy from a branch**, a branch **main** e a pasta **/(root)**. Salve.
4. Aguarde o endereço mostrado na página de configuração. Acesse o endereço mostrado em **Settings → Pages**, normalmente `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

Se aparecer 404, confirme que o nome é exatamente `index.html` (minúsculas), que ele está na raiz do repositório e que o Pages foi configurado para a mesma branch (`main`) e pasta (`/(root)`). O nome exibido no site continua o nome da Salles Transportes.

## Controle diário de passageiros MIP

A aba lateral **Passageiros MIP** reproduz a tabela da planilha enviada: TAG, cidade, início do bairro, os dias do mês com dia da semana e data, e a linha **TOTAL DO DIA**. Inclui MIP - 160 até MIP - 167 e TREINAMENTO. Cidade e bairro são editáveis por veículo; o número de passageiros pode ser preenchido em cada dia. O total diário é calculado automaticamente. Selecione o mês para consultar ou preencher outro período e use **Exportar mês CSV** para abrir a grade no Excel.

Os passageiros são salvos automaticamente no armazenamento local do navegador, separados por mês. A cópia JSON, a cópia HTML e o backup/restauração também incluem esses registros. Como o armazenamento continua local, a publicação no GitHub Pages não sincroniza automaticamente os dados entre computadores ou celulares; transfira uma cópia JSON para levar os registros a outro dispositivo.

## Como é feita a conta

No formulário de abastecimento, selecione o tipo de veículo (Ônibus, Microônibus, Van ou Carreta) e registre, se desejar, o nome do posto ao lado da localização. Tipo e posto também aparecem no histórico e na exportação CSV.

O histórico mostra o **KM anterior** do mesmo veículo, o **KM atual** e o **KM do período**:

`KM do período = hodômetro atual − hodômetro anterior`

Também mostra `km/L simples = KM do período ÷ litros do abastecimento atual`. Essa conta isolada pode distorcer o consumo quando o tanque não estava cheio ou quando houve abastecimentos parciais. Para a média medida, o aplicativo usa tanques cheios:

Para cada veículo, o primeiro abastecimento marcado **tanque cheio** inicia a medição. No próximo tanque cheio:

`km/L = (hodômetro atual − hodômetro do tanque cheio anterior) ÷ soma dos litros abastecidos depois do tanque cheio anterior, incluindo o abastecimento atual`

Se houve abastecimentos parciais, seus litros entram nessa soma. A média no resumo soma a distância de todos os intervalos completos e divide pela soma dos litros desses mesmos intervalos. O custo por km usa os preços registrados nesses intervalos. Um abastecimento parcial sem fechamento posterior não gera uma média medida. O primeiro registro de um veículo não tem KM anterior nem KM do período. Registros antigos continuam disponíveis; ao editá-los, preencha os novos campos.

Exemplo: tanque cheio aos 10.000 km; parcial de 20 L aos 10.200 km; tanque cheio de 30 L aos 10.500 km. Consumo medido: `500 ÷ (20 + 30) = 10 km/L`.

## Dados

Os registros ficam no armazenamento local do navegador e não são enviados ao GitHub. Abrir o site em outro aparelho ou navegador não traz os dados automaticamente. Se o navegador bloquear o armazenamento local, o aplicativo avisará e os dados valerão apenas até fechar ou recarregar a página. Use **Baixar cópia JSON** e **Restaurar cópia JSON** para transferir ou recuperar abastecimentos e passageiros. No hodômetro, `12.500` representa doze mil e quinhentos km; `12500.5` ou `12500,5` representa doze mil e quinhentos vírgula cinco km. Litros e preço aceitam vírgula ou ponto decimal, sem separador de milhar. **Baixar HTML com dados** cria uma cópia independente do aplicativo, com os registros daquele momento embutidos no próprio arquivo. Para incorporar alterações posteriores, baixe outro HTML e substitua a cópia antiga. Se a página for reaberta no mesmo navegador, alterações mais recentes também podem vir do armazenamento local; em outro dispositivo, o HTML começa pela cópia embutida. **Exportar CSV** inclui tipo de veículo, motorista, localização, posto, combustível, contrato, KM anterior, KM do período e as duas formas de km/L; produz um arquivo para Excel, com ponto e vírgula como separador e vírgula decimal.

## Versão desktop para Windows

A interface desktop usa **CustomTkinter** para exibir painéis, campos e botões arredondados em vermelho. Instale o Python 3 com suporte a Tkinter; coloque `abastecimento_desktop.py` e `iniciar_windows.bat` na mesma pasta e dê dois cliques no `.bat`. Na primeira execução ele instala automaticamente `customtkinter==6.0.0` pelo pip (precisa de internet apenas uma vez). Para instalar manualmente no **CMD**: `py -3 -m pip install --user "customtkinter==6.0.0"`. Depois rode `py -3 abastecimento_desktop.py`.

O campo obrigatório **TAG** fica à direita de **Nome do motorista**, é salvo no histórico, JSON e CSV. O histórico apresenta KM do período e Km/L simples; o cartão **Média medida** continua usando os intervalos entre tanques cheios. Dados anteriores continuam abrindo e, ao editar um registro antigo, preencha a TAG.

Os dados ficam em `%APPDATA%\ControleAbastecimento\dados.json` no Windows. Substituir o arquivo `.py` para atualizar a interface não apaga os dados. O programa não requer internet depois que a biblioteca estiver instalada.

Se quiser gerar um `.exe` no Windows, instale PyInstaller com `py -3 -m pip install pyinstaller` e rode `py -3 -m PyInstaller --onefile --windowed --collect-all customtkinter --name ControleAbastecimento abastecimento_desktop.py`. O executável aparecerá em `dist`.

### Se a versão web não aceitar cliques

Abra o arquivo `.html` em um navegador comum (ou publique no GitHub Pages). Visualizadores de arquivos que desativam JavaScript mostram o conteúdo, mas não executam os botões do aplicativo. Na versão web, a mensagem "Aplicativo pronto" confirma que o script foi iniciado.

## Acesso em vários locais

Publicar o HTML no GitHub Pages permite abrir o aplicativo em diferentes aparelhos, mas os registros ainda ficam no navegador de cada aparelho. O arquivo HTML com dados é uma cópia de um momento, não uma base compartilhada em tempo real. Para registros compartilhados automaticamente, será preciso conectar um banco de dados online com autenticação. Não envie para repositório público um HTML exportado com dados reais de motoristas ou contratos.
