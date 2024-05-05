# Desafio módulo 3 – Processamento de Dados Simplificado com Power BI


-- [Publicação no PowerBI Service](https://app.powerbi.com/links/pOLZO63WS_?ctid=9b00c2ab-b431-495d-85d8-6ffa844bf3e2&pbi_source=linkShare "Publicação no PowerBI Service")


## Transformação dos dados

### 1. Verificação dos cabeçalhos e tipos de dados
Os nomes de tabelas e colunas foram modificados para que os dados ficassem mais legíveis

### 2. Modificação dos valores monetários para o tipo double preciso
Tabela “Colaborador” - Coluna "Salário" - modificada para Número Decimal Fixo

### 3. Verificação da existência dos nulos e análise da remoção
Não julguei como necessária a remoção de nenhum nulo

### 4. Os Colaboradors com nulos em Gerente_ID podem ser os gerentes. Verifique se há algum colaborador sem gerente
James E. Borg não tem gerente, pois é o "gerente dos gerentes"

### 5. e 6.  Verifique se há algum departamento sem gerente. Se houver departamento sem gerente, suponha que você possui os dados e preencha as lacunas**
Não há

### 7. Verifique o número de horas dos projetos
Cálculo de quantas horas de trabalho geral cada projeto tem:

 - Mesclei tabela Projeto com Horas Trabalhadas pela coluna ProjetoID
 - Expandi a coluna da tabela Horas Trabalhadas, mantendo somente a coluna Horas 
 - Expandi a coluna da tab Departamentos mantendo somente a coluna Departamento, para manter o nome do dpto responsável pelo projeto 
 - Agrupei por - Avançadas - Colunas p/ agrupar: Projeto,    ProjetoID, Localização do projeto, Departamento
 - Nome da nova coluna: Total de Horas Trabalhadas - Operação: Soma - Coluna: Horas
 - A nova coluna tem a soma total de horas trabalhadas pelos colaboradores em cada projeto

### 8. Separar colunas complexas

Divisão da coluna Endereço p/ as colunas N° do endereço, Rua, Cidade e Estado, na tabela Colaborador:
- Dividir coluna dígito para não dígito à separou a coluna de número (col. N° de endereço)
- Dividir coluna por n° de caracteres - 2 caracteres - mais à direita à Separou a coluna Estado à direita
- Dividir por delimitador “-“ à deletei a coluna em branco, a rua Fire Oak também se separou, ficou a cidade Humble isolada em uma 3ª coluna
- Substituir valores da coluna Rua – Fire p/ Fire Oak
- Substituir valores da coluna Cidade – Oak p/ Humble
- Deletar a coluna onde Humble ficou isolado e a coluna em branco

### 9. Mesclar consultas Colaborador e Departamento para criar uma tabela Colaborador com o nome dos departamentos associados aos colaboradores. A mescla terá como base a tabela Colaborador. Fique atento, essa informação influencia no tipo de junção
- Achei mais útil deixar na mesma tabela,
- Selecionadas as tabelas Colaboradores (col DepartamentoID) e departmentos (DepartamentoID)
- Tipo de junção: Extrema esquerda
- Expande a tabela departmentos mantendo somente a coluna Departamento
- Por algum motivo desconhecido, ele duplicou várias linhas e tive que ir em “remover duplicadas”

### 10. Neste processo elimine as colunas desnecessárias.
- Deletei a DepartamentoID da tab Colaboradores, mantendo somente a de nome dos departamentos

### 11. Realize a junção dos colaboradores e respectivos nomes dos gerentes . Isso pode ser feito com consulta SQL ou pela mescla de tabelas com Power BI. Caso utilize SQL, especifique no README a query utilizada no processo.
- Também julguei mais apropriado deixar na mesma tabela ao invés de criar outra
- Mesclar – tabela Colaborador (col. GerenteID) + tabela Colaborador (col. ColaboradorID)
- Deletei a colunas GerenteID, mantendo somente a de nome Gerente

### 12. Mescle as colunas de Nome e Sobrenome para ter apenas uma coluna definindo os nomes dos colaboradores
- Selecionei as colunas de nome de gerente e de Colaborador
- Transformar – Mesclar colunas - Separador: Espaço, Nome das colunas: Gerente e Colaborador

### 13. Mescle os nomes de departamentos e localização. Isso fará que cada combinação departamento-local seja único. Isso irá auxiliar na criação do modelo estrela em um módulo futuro.
- Expandi a coluna da tabela Departmentos mantendo somente a coluna Departamento
- Adicionar coluna - coluna personalizada – nome: Departamento_Loc
-- Fórmula de coluna personalizada: [Departamento]&"_"&[ Localização do departamento]
- Deletei a coluna DepartamentoID

### 14. Explique por que, neste caso supracitado, podemos apenas utilizar o mesclar e não o atribuir.

- Porque nesse exemplo as tabelas têm estruturas diferentes, com colunas diferentes que devem ser mescladas a partir de uma coluna em comum.
- Para atribuir, é necessário que as tabelas tenham as mesmas colunas, geralmente na mesma ordem

### 15. Agrupe os dados a fim de saber quantos colaboradores existem por gerente
- Dupliquei a tabela Colaboradores, colocando o nome Gerentes
- Transformar – Agrupar por – Gerente – Contar linhas
- Remover linhas – 1 linha inferior (tinha ficado em branco)

### 16. Elimine as colunas desnecessárias, que não serão usadas no relatório, de cada tabela
- Departamentos: deletei as colunas das tabelas Colaboradores, projetos e localizações, GerenteID (substitui por uma com os nomes dos gerentes)
- Dependentes: deletei a coluna da tabela Colaborador, mantendo somente a de nomes. Deletei ColaboradorID
- Horas Trabalhadas: deletei a coluna da tabela Colaborador e mantive só a Projeto na coluna da tabela Projetos. Deletei ProjetoID e substituí o ColaboradorID pela coluna Colaboradores
- Projetos: deletei ProjetoID e DepartamentoID

```