# Audita — resultados do MVP

## Importação do Excel do BI

- [ ] A pessoa usuária consegue selecionar `.xlsx`, `.xls` ou `.csv` no painel.
- [ ] Antes de importar, a pessoa usuária seleciona se o arquivo é do tipo MQI ou PQI; o tipo fica registrado na importação, em cada pendência e no CSV exportado.
- [ ] A primeira linha é tratada como cabeçalho e cada linha seguinte vira uma pendência em sequência.
- [ ] Os dados necessários para a operação são reconhecidos quando os cabeçalhos usam nomes comuns de auditoria; no Excel de exemplo, `Id`, `Unique_ID`, `Photo`, `WF`, `Section`, `Severity`, `Description`, `Action`, `Due Date`, `Status` e `Causative` são preservados.
- [ ] Ao abrir uma pendência, todos os campos originais da linha aparecem no detalhe, incluindo links de foto e textos longos, sem depender apenas do resumo exibido na fila.
- [ ] Todo novo Excel exportado da mesma fonte padrão recebe automaticamente, em cada linha, a mesma seleção, filtro, categorização e padronização já aplicada: ID/código, `Unique_ID`, turbina, FY, descrição, WF/auditoria, Section/área, responsável, prazo, Severity/prioridade e Status; os dados originais permanecem preservados e IDs duplicados não são criados.
- [ ] O `Unique_ID` padrão é interpretado em três partes separadas por `_`: primeiro trecho = Projeto, segundo trecho = Turbina e terceiro trecho = Data; códigos como `SA05-1` devem ser exibidos como `SA05-01`, e a regra vale para todos os novos Excels da mesma fonte.
- [ ] A importação informa quantas linhas foram carregadas e evita criar pendência para linha completamente vazia.
- [ ] Existe uma ação protegida por confirmação para remover as pendências e o histórico associados ao arquivo de exemplo `data.xlsx`, sem remover outras importações.

## Fila colaborativa de pendências

- [ ] O painel mostra total, abertas, em correção e encerradas.
- [ ] A pessoa colaboradora consegue buscar e filtrar por status, responsável, prioridade, auditoria e prazo.
- [ ] Cada item exibe identificador sequencial, achado/título, auditoria, responsável, prazo, prioridade e status.
- [ ] O painel funciona em desktop e celular, com linguagem simples em português do Brasil.

## Correção e encerramento

- [ ] A pessoa colaboradora consegue editar responsável, prazo, prioridade, status, correção, evidência e comentário.
- [ ] O item só pode ser encerrado quando houver evidência, comentário e data de encerramento.
- [ ] Uma pendência encerrada pode ser reaberta para correção posterior.
- [ ] A evidência pode ser registrada como link ou descrição/localização do arquivo, sem obrigar upload nesta primeira versão.

## Histórico e compartilhamento

- [ ] Cada criação, edição, importação, encerramento e reabertura gera histórico com pessoa, data e resumo.
- [ ] O histórico pode ser consultado dentro do detalhe da pendência.
- [ ] O resultado atualmente filtrado pode ser exportado em CSV para compartilhar ou guardar.

## Persistência e acesso

- [ ] Os dados ficam persistidos no banco gerenciado do projeto.
- [ ] As operações de pendências exigem login Manus e usam o usuário autenticado como autor das alterações.

## Administração e históricos por exercício fiscal

- [ ] A pessoa autenticada que possui a senha administrativa consegue selecionar uma ou várias pendências e excluí-las; a exclusão remove também o histórico associado.
- [ ] A pessoa autenticada que possui a senha administrativa consegue excluir todas as pendências de uma vez; a ação remove também os históricos e as importações.
- [ ] A exclusão total exige um FY específico selecionado e remove somente as pendências e históricos daquele ano; com “Todos os exercícios”, o botão fica desabilitado.
- [ ] A limpeza do arquivo de exemplo `data.xlsx` também exige a senha administrativa.
- [ ] O exercício é calculado pelos seis últimos dígitos de `Unique_ID` — por exemplo, `Sao Vitor_SV12-01_25_01_17` pertence a `FY2025` — e a regra vale para pendências MQI e PQI.
- [ ] Ao selecionar um FY, a fila e os indicadores Total, Abertas, Em correção e Encerradas mostram somente os respectivos valores daquele ano; o FY também aparece na exportação.

## Evidências com imagem

- [ ] No campo Evidência, a pessoa colaboradora consegue escolher uma imagem JPG, PNG, GIF ou WEBP do computador, de até 10 MB.
- [ ] A imagem é enviada ao armazenamento durável pelo servidor e seu endereço é inserido automaticamente no campo Evidência; a pessoa precisa salvar a pendência para registrar a alteração.
- [ ] A imagem anexada aparece como miniatura na própria área de evidência e pode ser aberta em tamanho maior ao clicar.
- [ ] Prazo e prioridade são exibidos como somente leitura, permanecem iguais aos dados importados do Excel e não podem ser alterados por colaboradores nem pela API de atualização.
- [ ] A barra de pesquisa possui, ao lado de Responsável, um filtro de tipo com MQI, PQI e MQI + PQI; a escolha filtra a fila e os indicadores.
- [ ] O cabeçalho mostra a data atual em português e se atualiza automaticamente todos os dias, inclusive se o painel continuar aberto.
- [ ] A interface usa a paleta visual Siemens Energy e exibe o logotipo Siemens Energy na navegação do painel geral.

- [ ] A área de evidência aceita imagens, PDF, Excel (XLS/XLSX/CSV), Word e TXT de até 10 MB; imagens mostram miniatura e os demais arquivos mostram nome e link para abertura.

- [ ] O atalho Histórico abre um painel geral com as últimas alterações de todas as pendências, mostrando colaborador, ação, pendência, tipo, FY, data e descrição; o detalhe da pendência mantém seu histórico individual.

- [ ] A interface mantém todas as funcionalidades existentes e recebe acabamento profissional na hierarquia visual, indicadores, filtros, fila, navegação, estados de interação e responsividade.

- [ ] Cada pendência exibe seu identificador no formato ID248 no lugar da numeração sequencial 001/002/003; a barra permite filtrar por esse ID e a fila distribui melhor título, responsável, prazo e prioridade.

- [ ] O filtro de Auditoria é removido; a fila ordena vencidas primeiro em vermelho, próximas do vencimento em amarelo claro e pendências com prazo normal em verde claro.
- [ ] A importação não cria uma nova pendência quando o ID já existe no banco ou aparece repetido no mesmo arquivo; as linhas duplicadas são ignoradas e os IDs recusados são informados no resultado.

- [ ] A exportação das pendências filtradas gera XLSX; a primeira imagem da evidência aparece incorporada como miniatura e todos os links originais permanecem em coluna própria.

- [ ] Quando o título for genérico, a pendência exibe a turbina extraída do Unique_ID, como SV14-01; a descrição exibida vem do campo Descrição/Achado da respectiva linha.
- [ ] A fila possui filtro Turbina que localiza os identificadores SV**-** derivados do Unique_ID.
- [ ] Ao salvar uma pendência com status Encerrada, o detalhe fecha automaticamente, a visão geral permanece aberta e as encerradas ficam no final da fila.
