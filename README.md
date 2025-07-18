# Ontologia de Referência para Alergias Alimentares

Repositório com a finalidade de abrigar a documentação, a modelagem e a construção de uma ontologia de referência para descrever alergias alimentares e seus componentes.

# Estrutura

Este repositório contém os seguintes artefatos:

* **[Alergia_Alimentar_ORSD.pdf](./Alergia_Alimentar_ORSD.pdf):** Documento de Especificação de Requisitos de Ontologias (ORSD), detalha os objetivos, o escopo e as questões de competência que guiaram o desenvolvimento da ontologia.
* **[alergia-alimentar-padronizado.vpp](./alergia-alimentar-padronizado.vpp):** Projeto no [Visual Paradigm](https://www.visual-paradigm.com), contém a modelagem do diagrama de conceitos utilizando a linguagem OntoUML com padrões de projeto aplicados.
* **[alergia-alimentar.ttl](./alergia-alimentar.ttl):**  Arquivo de implementação da ontologia em formato Turtle, utilizando a Web Ontology Language (OWL). Pode ser aberto na ferramenta de edição de ontologias [Protégé](https://protege.stanford.edu).
* **Imagens para Visualização:**
    * **[alergia-alimentar.png](./alergia-alimentar.png):** Diagrama de classes com estereótipos [OntoUML](https://ontouml.org) focado no conceito de `Paciente`.
    * **[class-hierarchy.png](./class-hierarchy.png):** Print da hierarquia de classes da ontologia no Protégé, organizada conforme a especificação da [UFO](https://ontouml.org/ufo/)

# Conceitos Chave

Esta ontologia é construída sobre conceitos como `Paciente`, `Alimento`, `Alérgeno`, `Alergia`, `Sintoma`, `ProfissionalDeSaude`, `Diagnostico` e `Tratamento`. Ela visa ser um recurso valioso para a compreensão e o gerenciamento das alergias alimentares, proporcionando uma base sólida para sistemas de informação e pesquisa.

A estrutura da ontologia permite uma representação detalhada do domínio:

* **Alimentos e Componentes:** Um `Alimento` pode ser atômico (`Ingrediente`) ou composto por outros alimentos (`contem` outro `Alimento`), como uma salada de frutas. Além disso, possui subtipos como `Formula` (para fórmulas infantis) e `Ingrediente`.
* **Composição Química:** Todo `Alimento` é `composto_por` um ou mais `ComponenteAlimentar`, que possui subtipos como `Proteina`, `Carboidrato` e `AditivoAlimentar`.
* **O Papel do Alérgeno:** Um `ComponenteAlimentar` (como a proteína Albumina) pode desempenhar o papel de `Alérgeno` quando um `Paciente` específico possui uma `Alergia` a ele.
* **Eventos Clínicos:** O `ConsumoAlimentar` por um `Paciente` pode desencadear uma `ReacaoAdversa`, que por sua vez se manifesta através de `Sintoma`. A `ReacaoAdversa` pode ser influenciada por fatores de risco como `HerancaGenetica` ou `ComobidardeAlergica`.
* **Tipos de Alergia e Sintomas:** As alergias são classificadas em `ImunoMediada`, `NaoImunoMediada` (intolerâncias) e `Mista`. Os sintomas associados se dividem em `Cutaneo`, `Gastrointestinal`, `Respiratorio` e `Sistemico`.
* **Diagnóstico e Tratamento:** Um `ProfissionalDeSaude` pode realizar um `Diagnostico` utilizando `Procedimento`(s) como `TesteCutaneo`, `TesteDeDosagemIgESerica` ou `TesteDeProvocacaoOral`. Com base no diagnóstico, um `Tratamento` pode ser prescrito, incluindo `ImunoterapiaOral`, `Medicamento` ou `DietaDeExclusao`.

# Seção Técnica: Detalhes da Implementação em OWL

A ontologia foi implementada em OWL 2 para garantir expressividade e capacidade de inferência computacional. A seguir, é detalhado como os requisitos técnicos foram cumpridos.

### Conformidade com as Especificações OWL

A ontologia utiliza um rico conjunto de axiomas OWL para modelar o domínio de forma precisa.

* **Disjunção entre Classes:** Foram estabelecidas disjunções para garantir que um indivíduo não possa pertencer a mais de uma classe em um conjunto específico. Por exemplo, `Formula` e `Ingrediente` são disjuntos, assim como os subtipos de `Sintoma` (`Cutaneo`, `Gastrointestinal`, etc.).
* **Condições Necessárias e Suficientes:**
    * **Classes Primitivas (Condição Necessária):** A maioria das classes (ex: `Alimento`) é definida com `SubClassOf`, estabelecendo as condições que seus membros devem cumprir.
    * **Classes Definidas (Condição Necessária e Suficiente):** Classes como `Adulto` e `Crianca` são definidas com `EquivalentTo`. Isso permite que o inferenciador (reasoner) classifique automaticamente indivíduos nelas com base em suas propriedades (como a `idadePaciente`).
* **Axiomas de Fechamento:** A propriedade `contem` na classe `Alimento` é restrita para garantir que um alimento só possa conter outros indivíduos da classe `Alimento`, fechando o escopo da propriedade.
* **Classes Enumeradas:** A classe `NivelDeRisco` é definida como uma classe enumerada, limitando seus membros exclusivamente a `RiscoAlto`, `RiscoBaixo` e `RiscoMedio`.
* **Propriedades e Quantificadores:**
    * **Object e Data Properties:** Todas as propriedades têm domínios e imagens bem definidos (ex: `composto_por` liga `Alimento` a `ComponenteAlimentar`). Propriedades de dados, como `idadePaciente` e `dataDiagnostico`, utilizam tipos de dados `xsd:integer` e `xsd:date`, respectivamente.
    * **Características de Propriedades:** Propriedades foram caracterizadas para refinar sua semântica, como `Transitive` (para `contem`) e `Asymmetric` (para `tem_alergia_a`).
    * **Quantificadores de Cardinalidade:** A ontologia usa quantificadores (`some`, `only`, `min`, `max`, `exactly`) para restringir o número de relações que um indivíduo pode ter. Por exemplo, todo `Diagnostico` deve mediar (`mediates`) ao menos um `Paciente`.
* **Indivíduos:** A ontologia é populada com indivíduos exemplos, como `Maria` (`Paciente`), `Ovo` (`Alimento`) e `Albumina` (`Proteina`, `Alergeno`), que ilustram a aplicação prática do modelo.

### Padrões de Design de Ontologias

A modelagem foi guiada por padrões de design de ontologias fundamentados pela OntoUML, que possuem potencial de enriquecer a ontologia e ajudam a resolver problemas relacionados à modelagem.

* **Relator Pattern:** Utilizado para reificar (tratar como uma coisa) relações complexas e multivaloradas entre entidades. A existência do relator depende das entidades que ele conecta.
    * **Classes:** `Alergia`, `AvaliacaoDeRisco`, `Diagnostico`, `Tratamento`.
    * **Explicação:** A classe `Alergia`, por exemplo, não é uma entidade física, mas sim a materialização da relação entre um `Paciente` e um `Alérgeno`. Da mesma forma, `Diagnostico` reifica a relação entre `Paciente`, `ProfissionalDeSaude` e `Procedimento`.
* **Subkind Pattern:** Usado para especializar um tipo fundamental (`Kind`) ou outro `Subkind`. As subclasses herdam a identidade do seu supertipo, adicionando restrições, mas mantendo a rigidez (um `Subkind` não pode deixar de sê-lo).
    * **Classes:** `Proteina`, `Carboidrato` e `AditivoAlimentar` (como subtipos de `ComponenteAlimentar`); `Formula` e `Ingrediente` (de `Alimento`); `ImunoMediada` e `NaoImunoMediada` (de `Alergia`); `Cutaneo`, `Gastrointestinal`, `Respiratorio` e `Sistemico` (de `Sintoma`).
    * **Explicação:** Uma `Proteina` é sempre um `ComponenteAlimentar`. Ela não pode existir sem ser um `ComponenteAlimentar` e não pode deixar de ser `Proteina` durante sua existência.
* **Role Pattern:** Representa um papel que uma entidade desempenha em um contexto relacional. É uma classificação contingente: uma entidade pode começar a desempenhar um papel, e também deixar de desempenhá-lo, sem perder sua identidade.
    * **Classe:** `Alergeno`.
    * **Explicação:** Um `ComponenteAlimentar` (como a `Albumina`) não é inerentemente um `Alérgeno`. Ele assume esse papel (`role`) apenas para um `Paciente` específico no contexto de uma `Alergia`. Para outro `Paciente`, o mesmo componente pode ser inofensivo.
* **Phase Pattern:** Descreve uma fase ou estágio temporário na vida de uma entidade. Diferente de um `Subkind`, uma entidade pode transitar entre diferentes fases.
    * **Classes:** `Adulto`, `Crianca`.
    * **Explicação:** `Paciente` é um tipo fundamental (`Kind`). Um indivíduo da classe `Paciente` passa pela fase `Crianca` e depois transita para a fase `Adulto` com base em uma condição intrínseca (a `idadePaciente`). Um `Paciente` não é `Crianca` e `Adulto` ao mesmo tempo, e essa classificação pode mudar sem alterar sua identidade como `Paciente`.
* **Event and Situation Pattern:** Utilizado para modelar fenômenos dinâmicos em que um evento (`Event`) gera uma situação estática resultante (`Situation`), que por sua vez pode desencadear novos eventos. Este padrão segue a abordagem descrita no artigo "Representing the UFO-B Foundational Ontology of Events in SROIQ".
    * **Classes:** `ConsumoAlimentar` (`Event`), `ExposicaoAoAlergeno` (`Situation`), `ReacaoAdversa` (`Event`), `DisposicaoAlergica` (`Mode`).
    * **Explicação:** O evento de `ConsumoAlimentar` por um paciente gera (`bringsAbout`) a situação de `ExposicaoAoAlergeno`. Essa situação, por sua vez, pode contribuir para desencadear (`triggers`) um novo evento, a `ReacaoAdversa`, que manifesta (`manifestation`) uma `DisposicaoAlergica` (uma condição intrínseca) do paciente, juntamente acompanhada por 1 ou mais instâncias de `Sintoma` com (`historicalDependence`), pois um `Sintoma` só existe ao ocorrer uma `Reacao Adversa` de um paciente.

### Demonstração de Capacidade de Inferência

A estrutura da ontologia foi projetada para permitir que um reasoner OWL deduza novo conhecimento.

1.  **Classificação por Hierarquia:** Um indivíduo como `Albumina`, declarado como `Proteina`, é automaticamente inferido como sendo também um `ComponenteAlimentar`, pois `Proteina` é uma subclasse de `ComponenteAlimentar`.
2.  **Classificação por Condição (Fases):** O indivíduo `Carlos`, declarado como `Paciente` com `idadePaciente` igual a 25, é automaticamente classificado como `Adulto`, pois sua idade satisfaz a condição definida para a classe `Adulto` (idade >= 18).
3.  **Classificação por Composição (Conceitos Abstratos):** A ontologia pode inferir classificações complexas. Por exemplo, a `Caseina` é reclassificada como um `AlergenoPotencialmenteGrave` porque ela satisfaz uma cadeia de condições: ela é um alérgeno (`Alergeno`) que faz mediação à uma alergia (`AlergiaAoLeiteDaMaria`) em uma paciente (`Maria`) que possui um sintoma sistêmico (`EdemaLaringeo`).

# Seção de Modelagem: Justificativa das Escolhas com OntoUML

Esta seção detalha as escolhas de modelagem utilizando os estereótipos da linguagem OntoUML, que fundamentam a semântica da ontologia.

* **«kind»:** Representa os tipos fundamentais e rígidos.
    * **Exemplos:** `Paciente`, `Alimento`, `ProfissionalDeSaude`, `ComponenteAlimentar`, `Procedimento`.
    * **Justificativa:** Um `Paciente` é sempre um `Paciente`; sua identidade é fundamental no domínio.
* **«subkind»:** É uma especialização de um `«kind»` que herda sua identidade.
    * **Exemplos:** `Proteina` (de `ComponenteAlimentar`), `ImunoMediada` (de `Alergia`), `Respiratorio` (de `Sintoma`).
    * **Justificativa:** Um sintoma `Respiratorio` é, antes de tudo, um `Sintoma`, mas com uma especialização.
* **«role»:** Representa um papel que uma entidade desempenha em um contexto.
    * **Exemplo:** `Alergeno`.
    * **Justificativa:** Um `ComponenteAlimentar` só se torna um `Alergeno` no contexto de uma `Alergia` para um `Paciente` específico.
* **«relator»:** Define uma relação material e complexa cuja existência depende das entidades que ela conecta.
    * **Exemplos:** `Alergia`, `Diagnostico`, `Tratamento`.
    * **Justificativa:** Uma `Alergia` reifica (torna concreta) a relação entre um `Paciente` e um `Alérgeno`.
* **«event»:** Representa ocorrências ou acontecimentos no tempo.
    * **Exemplos:** `ConsumoAlimentar`, `ReacaoAdversa`.
    * **Justificativa:** O `ConsumoAlimentar` é o ato de comer em um momento específico, servindo como um evento no histórico do paciente.
* **«quality»:** Representa propriedades inerentes e quantificáveis de uma entidade.
    * **Exemplos:** `ComorbidadeAlergica`, `HerancaGenetica`.
    * **Justificativa:** São características intrínsecas de um `Paciente`, como ter asma ou uma predisposição genética.
* **«mode»:** Representa propriedades qualitativas que um indivíduo manifesta.
    * **Exemplo:** `Sintoma`.
    * **Justificativa:** Um sintoma como "urticária" não existe de forma independente; ele é uma manifestação em um `Paciente`.

# Referências

* [Especificação OntoUML](https://ontouml.readthedocs.io/en/latest/index.html)
* [Artigo: Como escrever documentos de especificação de requisitos de ontologias](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=9981af42782872ec341db7f5ff149e6e629f16ec)
* [Artigo: Consenso Brasileiro sobre Alergia Alimentar: 2018 - Parte 1 - Etiopatogenia, clínica e diagnóstico.](http://aaai-asbai.org.br/detalhe_artigo.asp?id=851)
* [Artigo: Consenso Brasileiro sobre Alergia Alimentar: 2018 - Parte 2 - Diagnóstico, tratamento e prevenção.](http://aaai-asbai.org.br/detalhe_artigo.asp?id=865)
* [Artigo: From Reference Ontologies to Ontology Patterns and Back](www.researchgate.net/publication/315442889_From_Reference_Ontologies_to_Ontology_Patterns_and_Back)
* [Artigo: Representing the UFO-B Foundational Ontology of Events in SROIQ](https://www.researchgate.net/publication/319902161_Representing_the_UFO-B_Foundational_Ontology_of_Events_in_SROIQ)
* [Artigo: “We Need to Discuss the Relationship”: Revisiting Relationships as Modeling Constructs](https://www.researchgate.net/publication/274637620_We_Need_to_Discuss_the_Relationship_Revisiting_Relationships_as_Modeling_Constructs)