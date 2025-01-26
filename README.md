# RAG_study
RAG study on `https://arxiv.org/pdf/2403.10131v2`

# Processus de mise en œuvre

Initialement, nous avons constaté que le Raft qui exécute ce projet nécessitait les API openai ou openai azure, et le coût de l'activation de ces services était plus élevé que prévu, nous nous sommes donc tournés vers la recherche de `raft_local`.

Cependant, le modèle de génération de questions (`t5-small`) utilisé par défaut dans craft_local ne pouvait tout simplement pas produire de phrases lisibles, même avec l'échantillon de données fourni par ce projet. Nous avons dû le remplacer par le modèle plus puissant [google/flan-t5-large](https://huggingface.co/google/flan-t5-large).

Les données de l'échantillon ont été passées avec succès. `raft_local` découpe effectivement le texte en morceaux et génère des questions et des distracteurs sur la base de ce texte.

Nous avons ensuite essayé d'évaluer les différentes versions du modèle Q&A en fonction de l'ensemble de données utilisé dans l'article. Comme nous ne disposions pas de l'api openai, nous n'avons pas pu utiliser GPT-3.5 comme contrôle. Cependant, lors de la mise en œuvre, nous avons constaté que le modèle utilisé dans le projet (LLaMA2-7B) n'était pas non plus disponible localement. [La version du projet](https://huggingface.co/gorilla-llm/gorilla-7b-hf-delta-v0) et [la version Meta](https://huggingface.co/meta-llama/Llama-2-7b) nécessitent toutes deux l'API. Nous avons donc dû nous contenter du modèle DSF (Domain-specific Fine Tuning).

Nous avons d'abord choisi l'ensemble de données [PubMedQA](https://huggingface.co/datasets/qiaojin/PubMedQA) pour l'évaluation. Comme il s'agit d'un ensemble de données médicales pertinentes, nous avons choisi ce modèle [HPAI-BSC/Llama3-Aloe-8B-Alpha](https://huggingface.co/HPAI-BSC/Llama3-Aloe-8B-Alpha) pour le tester en tenant compte de la puissance arithmétique dont nous disposons.

Cependant, même dans ce cas, notre mémoire ne nous permet de tester que des ensembles de données d'une longueur inférieure à 10, ce qui, tout en montrant la différence de correction entre RAG et le fait de laisser le modèle répondre directement (DSF et DSF + RAG), ne nous permet pas de voir la différence de performance entre RAG et RAFT.