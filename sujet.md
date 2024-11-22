# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. 25 février 1991, Dharan, Arabie Saoudite, 28 morts, 100 blessés causés par un missile iraquien sur une base américaine dans la triste et célèbre guerre du golf. 

- Mais que s'est-il passé ?   
Alors que la guerre bat sont plein depuis plus de 7 mois, un scud iraquien frappe la caserne de Dahran le 14e détachement de quartier-maître de l’armée américaine. Cependant, l'attaque aurait dû être déjouée par le système de radar Patriot, mais un bug logiciel au niveau de la gestion des horodatages a décalé la partie du ciel scruté par le radar. 

- Mais pourquoi était-il décalé ?  
Le problème vient de la batterie du radar, qui était en fonction depuis plus de 100 heures, et qui à cause d'une erreur d'accumulation d'arrondi sur le calcul du temps, a créé un retard de 0.3 secondes sur le temps réellement écoulé. Cette erreur a causé un décalage de 500 mètres sur la prédiction du missile irakien.

- Est ce que l'erreur aurait-il pû être trouvée ?  
Avec un plan de test, avec une durée un peu plus élargit, le probème aurait été découvert mais en temps de guerre les délais de fabrication était surement restraint par l'armée Américaine.

- Est que l'on qualifie ce bug de local ou global ?  
Ce bug était précisément une erreur d'arrondit sur le 24e Bit d'une séquence, de ce fait le bug est local et a pû être rapidement corriger.


Sources :  
    https://fr.wikipedia.org/wiki/Guerre_du_Golfe,   
    https://horustest.io/blog/les-10-bugs-informatiques-les-plus-couteux-de-l-histoire/,  
    https://math.univ-lyon1.fr/irem/Formation_ISN/formation_representation_information/nombre/codage_numeriques_des_nombres.html

2. Le bug du ticket 813 est local car il ne dépends que d'une erreur dans une méthode d'un composant. Il s'agit d'une différence entre la documentation et le comportement de la méthode. En effet celle-ci est sensée retourner une exception particulière si un des paramètres est null. Ceci peut entraîner d'autres bugs si la méthode est utilisée par une autre personne qui ne connait pas l'implémentation. La solution adoptée est l'ajout d'une vérification des paramètre en entrée avec ```Objects.requireNonNull``` qui retourne une exception si un des paramètres est null. La personne qui a corrigé ce bug à bien rajouté des tests à la suite de sa correction pour prévenir d'un futur bug.

- [Le commit](https://github.com/apache/commons-collections/commit/7eb78290c8d7d1fa379536700de0bd4a81320bb0#diff-27c19e081e1e90e79b36daef451dcb7b44c295b56d0575e4f63648d7f3d158dc)
- [Le ticket](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-813?filter=doneissues)

3. Netflix utilise une méthode de stimulation de panne pour tester l'ensemble de son système pour cela, ils utilisent la méthode de "Chaos Engineering".

- Quelles sont les expériences qu'ils effectuent ?  
Les expériences effectuées sont de tester l'application sous certaines conditions comme en enlevant un groupe de serveur, en simulant une panne de réseau, en déconnectant des disques durs ou encore en créant de la latence artificiel.

- Quelles sont les exigences pour ces expériences ?  
Les exigences pour ces expériences sont tout d'abord de créer une hypothèse sur le dérouler du test, ensuite d'utiliser des outils comme Chaos Monkey afin d'automatiser les perturbations et enfin d'observer le temps de réponse et le taux d'erreur.

- Quelles sont les variables qu'ils observent et quels sont les principaux résultats qu'ils ont obtenus ?  
La première variable étudier pour chaque test est de vérifier si le système est toujours en fonctionnement malgré les erreurs. La deuxième est de regarder s'il y a une dégradation du temps de réponse, ensuite de voir le temps de réparation de la panne pour enfin finir sur la question de l'impact de l'erreur sur les utilisateurs.

- Netflix est-elle la seule entreprise à effectuer ces expériences ?
Non, les entreprises comme Amazon, Google, Microsoft et Facebook utilisent se procéder pour tester leurs applications.

4. Les avantages d'une spécification formelle pour web assembly sont : 
- L'universalité et interopérabilité du language 
- Une meilleure vérification de la fiabilité et de la sécurité du language
- Faciliter les autres futures preuves formelles
- Une base pour l'optimisation
- Une meilleure évolution du language. En possèdant une spécification formelle du language, celui-ci en évoluant ne pourra pas dévier de son objectif principal tout en conservant ses bases formelles.

Selon nous tester le language même si il possède une spécification formelle prouvée reste nécessaire car même si la specification à été prouvée formellement, l'implémentation ne l'ai pas forcement. Il est aussi parfois plus complexe de prouver formellement l'implémentation, on prouve généralement seulement un sous-ensemble de propriétés.

5.
