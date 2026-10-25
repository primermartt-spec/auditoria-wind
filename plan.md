# Audita — plano do MVP

## Objetivo

Criar um painel web colaborativo, em português do Brasil, para transformar uma planilha exportada do BI em pendências de auditoria acompanháveis. A pessoa responsável importa um arquivo Excel; cada linha vira uma pendência em sequência, preservando os dados originais e adicionando campos operacionais para responsável, prazo, status, correção, evidência, comentário e data de encerramento. Colaboradores autenticados conseguem filtrar, atualizar e encerrar as pendências. Toda atualização gera um histórico legível e compartilhável.

## Escopo entregue nesta primeira versão

- Login com a autenticação Manus já preparada no projeto.
- Importação de `.xlsx`, `.xls` e `.csv` pelo navegador, usando a primeira aba da planilha.
- Escolha do tipo da auditoria antes da importação: MQI ou PQI, gravada em cada importação e pendência.
- Mapeamento flexível das colunas: os campos do BI são reconhecidos por nomes comuns, e todos os campos originais da linha ficam preservados e visíveis no detalhe da pendência em `dadosOriginais`.
- Todo Excel futuro exportado da mesma fonte padrão passa pelas mesmas regras de normalização individual: ID/código, `Unique_ID`, turbina, FY, descrição, WF/auditoria, Section/área, responsável, prazo, Severity/prioridade e Status; duplicidades são verificadas usando o ID/código original da própria linha.
- O `Unique_ID` padrão é interpretado em três partes separadas por `_`: primeiro trecho = Projeto, segundo trecho = Turbina e terceiro trecho = Data; códigos como `SA05-1` são normalizados para `SA05-01`.
- Criação em lote mantendo a ordem das linhas do arquivo e um identificador sequencial.
- Painel com contadores de total, abertas, em correção e encerradas.
- Busca e filtros por status, responsável, prioridade, auditoria e prazo.
- Lista responsiva com identificação, título/achado, auditoria, responsável, prazo, prioridade e status.
- Detalhe editável com correção, evidência (link ou nome/descrição), comentário, responsável, prazo e data de encerramento.
- Encerramento exige evidência, comentário e data; reabertura também é possível.
- Histórico por pendência com ator, ação, data e resumo da alteração.
- Exportação do resultado filtrado em CSV para compartilhar ou guardar.
- Limpeza protegida das pendências importadas especificamente de `data.xlsx`, com confirmação antes da exclusão.

## Decisões de implementação

A aplicação usa o starter React + Express + tRPC + Drizzle já inicializado, com o banco MySQL gerenciado do projeto. As tabelas novas serão `audit_imports`, `audit_items` e `audit_item_history`; `audit_items.originalData` guarda o JSON das colunas originais do BI sem descartar informação desconhecida. As operações de listagem, importação, atualização e histórico serão procedimentos tRPC protegidos pelo login. O navegador lê a planilha com SheetJS e envia uma lista normalizada ao servidor, evitando upload de arquivo no primeiro MVP e mantendo a solução simples e barata.

O estado do produto é compartilhado por todo o projeto autenticado. Nesta etapa, não haverá convite por e-mail, comentários em tempo real, anexos binários ou notificações automáticas; a evidência será registrada como texto ou link para a localização do arquivo (Drive, SharePoint, rede interna etc.). Esses pontos ficam como evolução segura após validar o fluxo com a equipe.

## Estrutura de projeto

- `client/src/pages/Home.tsx`: painel, filtros, importação, detalhe e histórico.
- `client/src/index.css`: sistema visual e responsividade.
- `client/src/components/ui/`: componentes reutilizáveis já fornecidos pelo starter.
- `server/routers.ts`: API tRPC de pendências, importação, edição, encerramento e histórico.
- `server/db.ts`: consultas e gravações persistentes.
- `drizzle/schema.ts`: tabelas e tipos do domínio.
- `drizzle/*.sql`: migração versionada do banco.
- `public/manus-routes.json`: manifesto de rotas do site.

## Direção de design

### Alternativas consideradas

- Oficina Clara — painel editorial, luz natural e foco em leitura rápida (0,07).
- Centro de Controle — contraste escuro e linguagem operacional, pensado para telas de acompanhamento (0,06).
- Papel de Auditoria — textura e cores de formulário para lembrar checklists físicos (0,03).

### Direção escolhida: Oficina Clara

**Movimento:** editorial de operações, inspirado em cadernos de campo e dashboards de trabalho bem impressos.

**Princípios:** tornar o próximo passo óbvio; dar prioridade à leitura das pendências; separar estado de contexto; registrar confiança por meio de histórico visível.

**Filosofia de cor:** base marfim e grafite para reduzir cansaço em listas longas; azul-petróleo como cor proprietária de ação e navegação; âmbar para atenção e verde sálvia para encerramento, sem depender apenas de cor para comunicar estados.

**Paradigma de layout:** barra lateral estreita com índice do trabalho, cabeçalho de contexto e uma faixa de resumo que leva diretamente à fila operacional; a tabela se transforma em cartões no celular.

**Elementos de assinatura:** marca “A” dentro de um selo circular; etiquetas de status com pequena linha de progresso; painel lateral de detalhes que mantém a lista visível.

**Interação:** filtros são compactos e persistentes; ações de alto impacto têm linguagem explícita; salvar e encerrar dão retorno imediato; o histórico aparece como uma linha do tempo, não como log técnico.

**Animação:** entradas curtas e discretas, 160–220 ms; números do resumo atualizam suavemente; o painel de detalhe desliza pela lateral em desktop e vira tela/modal em mobile; sem animações decorativas durante importação.

**Tipografia:** Manrope para títulos e navegação, IBM Plex Sans para dados e textos longos. Hierarquia com títulos compactos, números grandes nos resumos e metadados menores porém legíveis.

**Essência da marca:** “Menos pendência perdida, mais correção comprovada.” Personalidade: clara, responsável, prática.

**Voz:** direta, acolhedora e sem jargão. Exemplos: “Importe o relatório e organize o próximo passo.” / “Feche somente quando a correção estiver comprovada.”

**Marca:** wordmark “audita” em minúsculas, com o “a” inicial tratado como selo de verificação; assinatura visual baseada em um círculo aberto que sugere acompanhamento, não burocracia.

**Cor proprietária:** azul-petróleo `#0F5C5E`.

## Limitações e evolução

O primeiro MVP deve funcionar para validar o processo com uma planilha real e alguns colaboradores. O Excel precisa ter cabeçalho na primeira linha; arquivos muito grandes poderão exigir processamento em lotes em uma etapa posterior. Todos os campos da linha, incluindo links de foto, ação, causador, seção e status do BI, aparecem no detalhe original; a evidência de encerramento da equipe ainda será um campo de texto/link, não um upload binário. Depois da validação, as próximas evoluções naturais são anexos, convite por equipe, permissões por auditoria, notificações e relatórios compartilháveis por link.

## Extensões administrativas

Cada pendência recebe um exercício fiscal no formato `FY2025`, `FY2026` etc. calculado pelos seis últimos dígitos do `Unique_ID` (`25_01_17` vira `FY2025`); MQI e PQI permanecem juntos no filtro. A fila e os quatro indicadores do painel usam esse mesmo recorte anual. Exclusões exigem login e a senha administrativa protegida do projeto, que funciona como autorização exclusiva para essa ação. A pessoa responsável pode selecionar pendências específicas, excluir todas ou limpar o arquivo de exemplo `data.xlsx`; os históricos associados também são removidos nessa ação.
O comando de exclusão total só fica habilitado quando um FY específico está selecionado e remove exclusivamente as pendências daquele ano, mantendo os demais exercícios intactos.

Na área de evidência, a pessoa colaboradora pode selecionar uma imagem JPG, PNG, GIF ou WEBP do computador, com até 10 MB. A imagem é enviada pelo servidor para o armazenamento durável do projeto e o endereço estável é inserido no campo de evidência para ser salvo junto com a pendência.
Quando o campo contém uma imagem anexada, o painel exibe uma miniatura imediatamente abaixo do texto; ao clicar, a imagem abre em tamanho maior.
Prazo e prioridade são campos de origem do Excel: aparecem no detalhe como somente leitura e o servidor ignora tentativas de alterá-los na atualização da pendência.
Na barra de pesquisa, o filtro de tipo ao lado de Responsável permite ver somente MQI, somente PQI ou ambos; a seleção também atualiza a fila e os indicadores.
O cabeçalho usa a data atual em português, atualizada automaticamente enquanto o painel permanece aberto. A navegação usa a paleta visual Siemens Energy e exibe o logotipo no painel geral.
Além de imagens, a área de evidência aceita PDF, Excel (XLS/XLSX/CSV), Word e TXT, com limite de 10 MB por arquivo. Imagens continuam com miniatura; os demais arquivos aparecem com nome e link para abrir.
O atalho Histórico abre uma visão geral com as últimas alterações de todas as pendências, mostrando colaborador, ação, pendência, tipo, FY, data e descrição. O detalhe de cada pendência continua mostrando somente o histórico daquele item.
Foi aplicada uma camada visual profissional ao dashboard, com maior hierarquia de informação, cartões com elevação sutil, filtros mais claros, fila mais legível, navegação lateral refinada, estados de interação e responsividade preservada.
Na fila, a identificação exibida usa o ID da pendência no formato `ID248` em vez da sequência `001`; a barra possui filtro exclusivo por ID e as colunas foram redistribuídas para equilibrar título, responsável, prazo e prioridade.
O filtro de Auditoria foi removido. A fila agora classifica automaticamente as pendências por prazo: vencidas primeiro em vermelho, vencimento nos próximos dois meses em amarelo claro e prazo normal acima desse intervalo em verde claro.
Na importação, o sistema normaliza os IDs, compara com os já cadastrados e também detecta repetições dentro do próprio arquivo; duplicatas não são criadas e seus IDs são informados no retorno da operação.
A exportação das pendências filtradas agora gera XLSX. A primeira imagem de evidência de cada item é incorporada como miniatura na planilha, enquanto todos os links originais permanecem em uma coluna própria; PDFs e demais documentos continuam disponíveis por link.
Quando o título importado for genérico, a interface usa a turbina extraída do `Unique_ID` no formato `SV14-01`; a descrição apresentada vem do campo de descrição da própria linha. A fila oferece filtro por turbina, fecha o detalhe automaticamente ao encerrar e posiciona pendências encerradas no final.
