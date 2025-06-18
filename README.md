# Ontologia de Referência para Alergênicos Alimentares
Repositório com finalidade de abrigar documentação, modelagem e construção de uma ontologia de referência para descrever alergênicos alimentares.

# Estrutura
Este repositório contém os seguintes documentos:
 * Documento de Especificação de Requisitos de Ontologias (ORSD)
 * Projeto no Visual Paradigm para modelagem do diagrama de conceitos usando OntoUML
 * Imagens para melhor visualização das visões do modelo:
    * Visão de Prontuário: principal visão do modelo, tem Paciente como conceito principal.
    * Visão de Sintoma: tem como conceito principal Sintoma e suas relações com Alergia e Alimento.
    * Visão de Diagnóstico: relaciona Diagnóstico realizado por um Profissional de Saúde através de um Procedimento para um Paciente.
    * Visão de Tratamento: relaciona os tipos de tratamento passados a um Paciente através de um Profissional de Saúde.

# Conceitos Chave
Esta ontologia possui conceitos como Paciente, Alimento, Alérgeno, Alergia, Sintoma, Profissional de Saúde, Diagnóstico e Tratamento.  
Ela visa ser um recurso valioso para a compreensão e gerenciamento das alergias alimentares, proporcionando uma base sólida para sistemas de informação e pesquisa no futuro.  
A ontologia começa com a introdução de alimentos e seus componentes, um alimento pode ser composto por outro alimento, por exemplo, salada de frutas é composta por banana, maçã, etc.  
Possui subtipos como Formula para fórmulas infantis e Ingrediente.  
Possui uma relação de composição com Componente Alimentar, que possui subtipos como Proteina, Carboidrato e Aditivo Alimentar, fazendo com que seja possível identificar possíveis alérgenos presentes nos alimentos.  
Componentes Alimentares, por sua vez, podem desempenhar o papel de Alérgeno para pacientes que consomem determinados alimentos.  
O ato de consumir um alimento pode desencadear uma Reação Adversa em um Paciente, que pode estar relacionada com fatores de risco, como Herança Genética, Comobidarde Alérgica, Fator Dietético ou Fator Cultural.  
Alergias possuem 3 tipos, Imuno Mediada (alergia alimentar e doença celíaca), Não Imuno Mediada (intolerância alimentar) e Mista.  
São acompanhadas por Sintomas dos tipos Cutâneo (pele), Gastrointestinal, Respiratório e Sistêmico (todo o corpo).  
Baseados nos Sintomas apresentados pelo Paciente, ele pode ser acompanhado por um Profissional de Saúde que vai realizar um Diagnóstico baseado nos seguintes procedimentos: Teste Cutâneo (aplica-se alérgenos na pele), Teste de Dosagem IgE Sérica (aplica-se anticorpos imunoglobulina E no sangue em laboratório) e Teste de Provocação Oral TPO (paciente consome pequenas quantidades do alérgeno suspeito).  
O Profissional de Saúde então prescreverá um Tratamento ao paciente, podendo incluir Imunoterapia Oral, Medicamento(s) e/ou uma Dieta de Exclusão.

# Seção Técnica: Justificativa das Escolhas de Modelagem
Esta seção detalha as escolhas de modelagem utilizando os estereótipos da linguagem OntoUML.  
 * «kind»: Representa os tipos fundamentais e rígidos de entidades no domínio, que possuem identidade própria e são a "essência" das coisas.  
    Exemplos: Paciente, Alimento, Profissional de Saúde, Componente Alimentar, Procedimento, Sintoma (o tipo geral do sintoma).  
    Justificativa: Um Paciente é sempre um Paciente; um Alimento é sempre um Alimento. Eles existem por si só.  
 * «subkind»: É uma especialização de um «kind» (ou outro provedor de identidade).  
   Herda a identidade de seu supertipo e adiciona uma restrição.  
   Exemplos: Proteína (de Componente Alimentar), Fórmula (de Alimento), Imuno Mediada (de Alergia), Teste Cutâneo (de Procedimento), Respiratório (de Sintoma).  
   Justificativa: Um Sintoma Cutâneo (ex: coceira, bolhas) é fundamentalmente um Sintoma, mas com a restrição de ser na pele.  
 * «role»: Representa papéis que uma entidade pode desempenhar em um determinado contexto ou em uma relação, não sendo intrínsecos à sua natureza fundamental e podendo mudar.  
   Exemplo: Alérgeno.  
   Justificativa: Uma Proteína/Carboidrato/Aditivo Alimentar se torna um Alérgeno para um paciente específico quando há uma Alergia, nem todo Paciente terá aquele Componente Alimentar como Alérgeno.  
 * «relator»: Define uma relação complexa e material entre duas ou mais entidades. A existência do Relator depende da existência dessas entidades.  
   Exemplos: Alergia, Diagnostico e Tratamento.  
   Justificativa: Uma Alergia é a reificação da relação entre um Paciente e um Alérgeno. Um Diagnóstico reifica a relação entre um Paciente, um Profissional de Saúde e Procedimentos. Um Tratamento reifica o plano terapêutico para um Paciente por um Profissional de Saúde.  
 * «event»: Representa ocorrências ou acontecimentos no tempo que não persistem.  
   Exemplos: Consumo Alimentar, Reação Adversa e Reação Cruzada.  
   Justificativa: Consumo Alimentar é o ato de comer em um momento específico, o Paciente inicia esse evento, servindo muito bem como histórico. Reação Adversa é o episódio clínico que se segue. Reação Cruzada é um tipo/especialização da Reação Adversa, elas podem ocorrer entre os alimentos devido à similaridade de uma determinada sequência de aminoácidos presente em um alergênico.  
 * «quality»: Representa propriedades estáveis, inerentes ou quantificáveis de uma entidade.  
   Exemplos: Comorbidade Alérgica e Herança Genética.  
   Justificativa: São características ou atributos do Paciente. Paciente x possui asma, rinite alérgica ou dermatite atópica, por exemplo.  
 * «mode»: Similar a <\<quality>\> mas representa propriedades intrínsecas qualitativas de uma única entidade.  
   Exemplos: Sintoma, Fator Cultural e Fator Dietético.  
   Justificativa: Um Sintoma (ex: urticária) não existe independentemente, mas sim em relação a um paciente que o apresenta. Não define o paciente, o paciente não é caracterizado pelo sintoma em si, mas sim está passando por um. Semelhante a Sintoma, um Fator Cultural (ex: exposição cultural a alimentos do mar, como crustáceos) ou Fator Dietético (ex: consumo elevado de ultraprocessados) representa um padrão de comportamento/contexto que é intrínseco a um Paciente (ou ao seu ambiente) e que influencia em sua saúde.  
 * «category»: Usado para classificar entidades de tipos ontológicos diferentes com base em um critério comum.  
   Exemplo: Risco.  
   Justificativa: Risco agrupa Fatores Culturais («mode»), Fatores Dietéticos («mode»), Comorbidades Alérgicas («quality») e Herança Genética («quality»), que são de tipos diferentes mas compartilham a característica comum de serem fatores de risco para uma Reação Adversa.  

# Referências

 * [Especificação OntoUML](https://ontouml.readthedocs.io/en/latest/index.html)
 * [Artigo: Modelagem de conceitos com o estereótipo <\<event>\>](https://www.inf.ufes.br/~gguizzardi/Events_as_Entities_in_Ontology-Driven_Co.pdf)
 * [Artigo: Como escrever documentos de especificação de requisitos de ontologias](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=9981af42782872ec341db7f5ff149e6e629f16ec)
 * [Artigo: Alergia Alimentar e o Cenário Regulatório No Brasil](https://revistas.ufg.br/REF/article/view/43433)
 * [Artigo: Consenso Brasileiro sobre Alergia Alimentar: 2018 - Parte 1 - Etiopatogenia, clínica e diagnóstico.](http://aaai-asbai.org.br/detalhe_artigo.asp?id=851)
 * [Artigo: Consenso Brasileiro sobre Alergia Alimentar: 2018 - Parte 2 - Diagnóstico, tratamento e prevenção.](http://aaai-asbai.org.br/detalhe_artigo.asp?id=865)
