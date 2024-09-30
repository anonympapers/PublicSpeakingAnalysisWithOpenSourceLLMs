# PublicSpeakingAnalysisWithLLMs
This is automatic public speaking analysis framework based on open-source Large Language Models (LLMs). This repository uses ollama library to sent requests through curl. 

## Table of Contents

- [Installation](#installation)
- [Pipeline](#pipeline)
- [Results](#results)
- [Model Parameters Used in Experiments](#modelparametersusedinexperiments)


## Installation

Once you clones this repository, install requirements from the requirements file:

```bash
# Install dependencies
pip3 install -r requirements.txt
```

Or directly create conda environment:
```bash
# Install dependencies
conda env create -f environment.yml
```

We used conda version 23.10.0 on the machine with following properties:

Linux Precision-3581 6.5.0-1024-oem
Architecture: x86_64
CPU: 13th Gen Intel(R) Core(TM) i9-13900H, 20 CPUs, 14 cores
Ubuntu 22.04.4 LTS

Than natigate to the ./analysis_configuration.sh file to check the system setup. There you can manipulate which subjective dimensions you would like to evaluate with LLMs or which criteria. 

## Pipeline

- Set Variables in ./analysis_configuration.sh file.
- Add your dataset to the ./data folder. Please, check the format of the other datasets to make sure that it is the same.
- Create prompts based on the transcripts that you provide:
    - For dimension evaluation:

```bash
# Initialise .sh file
chmod +x dimension_prompt_creation.sh

# Execute .sh file
./dimension_prompt_creation.sh
```
You should obtain prompts in the folder: ./prompts/$dataset/$dimension/$prompt_version/$clip/

    - For criteria evaluation:

```bash
# Initialise .sh file
chmod +x criteria_prompt_creation.sh

# Execute .sh file
./criteria_prompt_creation.sh
```
You should obtain prompts in the folder: ./prompts/$dataset/$criteria_category/$criteria/$prompt_version/$clip/

- Now you can exacute the LLM evaluation:

```bash
# Initialise .sh file for dimension evaluation
chmod +x dimension_analysis_with_LLM.sh

# Execute .sh file for dimension evaluation
./dimension_analysis_with_LLM.sh

# Initialise .sh file for criteria evaluation
chmod +x criteria_analysis_with_LLM.sh

# Execute .sh file for criteria evaluation
./criteria_analysis_with_LLM.sh

```

- Open-source models return answers in the .json format one file per prompt. In our case each transcript had corresponding prompt. Therefore, answers has to be aggregated, use this code:

```bash
# This code aggregated the scores
python3 aggregare_raw_outputs.py 
```

- Finally, not all of the prompts gave us the answer in the form of the one value. To extract the scores from the text provided by LLM we used scapy library and following script:

```bash
# This code aggregated the scores
python3 score_extraction_with_spacy.py 
```


## Criteria for Public Speaking Evaluation

| **Criteria**          | **Text**                                                                                                                                                                                                                                 |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Topic Presentation** | **La question :** À votre avis, comment évaluez-vous la présentation du sujet? <br> **Options de réponse :** <br> A: Problématique liée au sujet mal expliquée. <br> B: Explication acceptable de la problématique liée au sujet. <br> C: Très bonne explication de la problématique liée au sujet. |
| **Structure**          | **La question :** À votre avis, dans quelle mesure le discours de cette transcription est-il bien structuré? <br> **Options de réponse :** <br> A: Difficulté à comprendre les points clés, discours non argumenté. Discours décousu avec manque de transitions. <br> B: Points clés présents mais raisonnement lacunaire. Structure partiellement suivie, efficacité minimale des transitions. <br> C: Points clés valorisés et appuyés sur des arguments précis et pertinents. Plan structuré avec introduction, corps et conclusion, transitions efficaces. |
| **Language Level**     | **La question :** À votre avis, dans quelle mesure le niveau de langue du discours dans cette transcription est-il approprié, l'utilisation des mots ciblant le public est-elle appropriée? <br> **Options de réponse :** <br> A: Utilisation intensive de jargon, d'argot, de termes sexistes/racistes, ou vocabulaire limité. <br> B: Langage approprié, usage limité de jargon ou d'argot, vocabulaire suffisant pour être précis. <br> C: Langage approprié avec un vocabulaire étendu, y compris des expressions idiomatiques, une expression captivante, imaginative et vivante. |
| **Passive Voice**      | **La question :** À votre avis, comment évaluez-vous l'utilisation de la voix passive dans le discours de cette transcription ? <br> **Options de réponse :** <br> A: Un peu de voix passive pour la clarté/l'objectivité. <br> B: La voix passive suffit à rendre le texte clair. <br> C: La voix passive excessive qui rend le texte peu clair. |
| **Length**             | **La question :** Comment évaluez-vous la longueur des phrases du discours de cette transcription? <br> **Options de réponse :** <br> A: Phrases courtes qui rendent le texte moins clair. <br> B: Phrases de longueur suffisante pour rendre le texte clair. <br> C: Phrases excessivement longues qui rendent le texte peu clair. |
| **Redundancy**         | **La question :** À votre avis, quel est le degré de redondance du contenu du discours dans cette transcription? <br> **Options de réponse :** <br> A: Peu de contenu redondant ce qui rend le texte moins clair (ex. une inconsistance dans le choix des termes). <br> B: La redondance du contenu permet de rendre le texte clair. <br> C: Trop de redondance dans le contenu/répétition des mots, ce qui rend le texte moins clair. |
| **Negative Language**  | **La question :** Comment évaluez-vous l'utilisation de mots/expressions négatives dans le discours de cette transcription ? (mots négatifs : mauvais, tristes, etc. expressions négatives : ce n'est pas la pire solution, etc.) <br> **Options de réponse :** <br> A: Pas d'utilisation significative de mots négatifs. <br> B: Remarquée, mais pas d'impression négative. <br> C: Remarquée, et cela a donné une mauvaise impression. |
| **Metaphor**           | **La question :** Comment évaluez-vous l'utilisation de métaphores dans le discours de cette transcription? <br> **Options de réponse :** <br> A: Pas d'utilisation de métaphores. <br> B: La métaphore est utilisée, mais le lien avec le contexte du discours n'est pas évident ou la métaphore ne contribue pas à rendre l'explication plus mémorable et compréhensible. <br> C: La métaphore capture efficacement les détails du discours, renforce et facilite l'explication. |
| **Storytelling**       | **La question :** À votre avis, évaluez-vous l'utilisation d'une histoire/un exemple personnel dans le discours de cette transcription ? <br> **Options de réponse :** <br> A: Pas d'histoire/un exemple personnel. <br> B: L'histoire/un exemple personnel est présente mais pas utile pour rendre le discours plus captivant. <br> C: Utilisation pertinente de l'histoire/un exemple personnel qui rend le discours plus captivant. |

# Results

This directory contains results that we obtained for the LLaMA2, LLaMA3, Mistral models for the persuasiveness analysis and criteria evaluation with LLMs on [3MT_French dataset ](https://lineact.cesi.fr/en/publications/introducing-the-3mt_french-dataset-to-investigate-the-timing-of-public-speaking-judgements/). The results saved under the different names than it is described in the provided scripts. This happen due to the different version of this repository. Meanwhile, different names does not change the content and presented results. 

# Model Parameters Used in Experiments

| **Model**      | **Parameters**                               |
|----------------|----------------------------------------------|
| **LLaMa2**     | temperature: 0.9 (in [0, 1]) <br> top\_p: 0.6 |
| **Mistral**    | temperature: 0.7 (in [0, 1.5]) <br> top\_p: 1 |
| **LLaMa3**     | temperature: 0.6 (in [0, 1]) <br> top\_p: 0.9 |
| **GPT-4o-mini**| temperature: 1 (in [0, 2]) <br> top\_p: 1     |

## Average Metrics for Models and Prompts on Test Data

**Accuracy of LLM prediction of subjective scores.**  
$\color{red}{\downarrow}$ means that smaller metric values indicate more accurate predictions, while $\color{red}{\uparrow}$ means higher values are better.

| **LLM**        | **ID** | **MAE** $\color{blue}{\downarrow}$ $\color{red}{\uparrow}$ | **MSE** $\color{blue}{\downarrow}$ $\color{red}{\uparrow}$ | **RMSE** $\color{blue}{\downarrow}$ $\color{red}{\uparrow}$ | **R²** $\color{red}{\uparrow}$ $\color{red}{\uparrow}$ | **MAPE** $\color{blue}{\downarrow}$ $\color{red}{\uparrow}$ | **MedAE** $\color{blue}{\downarrow}$ $\color{red}{\uparrow}$ |
|----------------|-------|-----------------------------------------------------------|------------------------------------------------------------|------------------------------------------------------------|---------------------------------|------------------------------------------------------------|--------------------------------------------------------------|
| **LLaMA2**     | **01** | 0.818                                                     | 0.964                                                      | 0.982                                                      | -2.032                          | 19.99                                                      | 0.708                                                        |
| **Mistral**    | **01** | 0.683                                                     | 0.746                                                      | 0.864                                                      | -1.347                          | 18.226                                                     | 0.614                                                        |
| **LLaMA3**     | **01** | 0.606                                                     | 0.662                                                      | 0.814                                                      | -1.066                          | 15.866                                                     | 0.435                                                        |
| **GPT-4o-mini**| **01** | 1.535                                                     | 2.87                                                       | 1.694                                                      | -8.637                          | 37.022                                                     | 1.524                                                        |
| **LLaMA2**     | **02** | 0.755                                                     | 0.873                                                      | 0.926                                                      | -1.401                          | 19.137                                                     | 0.64                                                         |
| **Mistral**    | **02** | 0.688                                                     | 0.767                                                      | 0.873                                                      | -1.158                          | 18.599                                                     | 0.455                                                        |
| **LLaMA3**     | **02** | 0.597                                                     | 0.516                                                      | 0.718                                                      | -0.471                          | 15.681                                                     | 0.521                                                        |
| **GPT-4o-mini**| **02** | 1.233                                                     | 1.971                                                      | 1.404                                                      | -5.617                          | 29.667                                                     | 1.194                                                        |
| **LLaMA2**     | **03** | 0.663                                                     | 0.664                                                      | 0.815                                                      | -0.854                          | 17.026                                                     | 0.497                                                        |
| **Mistral**    | **03** | 0.892                                                     | 1.113                                                      | 1.051                                                      | -1.738                          | 22.9                                                       | 0.875                                                        |
| **LLaMA3**     | **03** | 1.143                                                     | 2.969                                                      | 1.724                                                      | -7.359                          | 29.521                                                     | 0.642                                                        |
| **GPT-4o-mini**| **03** | 1.621                                                     | 3.118                                                      | 1.766                                                      | -9.47                           | 39.224                                                     | 1.691                                                        |
| **LLaMA2**     | **04** | 0.957                                                     | 1.251                                                      | 1.117                                                      | -3.128                          | 23.187                                                     | 0.791                                                        |
| **Mistral**    | **04** | 0.846                                                     | 1.23                                                       | 1.104                                                      | -2.323                          | 22.37                                                      | 0.623                                                        |
| **LLaMA3**     | **04** | 0.59                                                      | 0.682                                                      | 0.821                                                      | -1.133                          | 15.347                                                     | 0.41                                                         |
| **GPT-4o-mini**| **04** | 1.359                                                     | 2.32                                                       | 1.523                                                      | -6.79                           | 32.753                                                     | 1.359                                                        |


## Self-Agreement of Models on Test Data

**ICC for LLaMA2, Mistral, and LLaMA3 models.**  
Each run `id` of each model `m` is considered a rater.

| **Group**    | **ID** | **ICC<sub>3,1</sub>** | **CI95%<sub>3,1</sub>**  | **ICC<sub>3,k</sub>** | **CI95%<sub>3,k</sub>**  |
|--------------|--------|----------------------|--------------------------|-----------------------|--------------------------|
| **LLaMA2**   | **01** | 0.39                 | [0.21, 0.57]              | 0.66                  | [0.44, 0.8]               |
| **Mistral**  | **01** | 0.73                 | [0.61, 0.83]              | 0.89                  | [0.82, 0.94]              |
| **LLaMA3**   | **01** | 0.40                 | [0.12, 0.62]              | 0.57                  | [0.21, 0.77]              |
| **GPT-4o-mini** | **01** | 0.22                 | [0.15, 0.29]              | 0.53                  | [0.41, 0.62]              |
| **LLaMA2**   | **02** | 0.20                 | [0.09, 0.31]              | 0.43                  | [0.25, 0.57]              |
| **Mistral**  | **02** | 0.54                 | [0.35, 0.7]               | 0.78                  | [0.62, 0.88]              |
| **LLaMA3**   | **02** | 0.37                 | [0.19, 0.56]              | 0.64                  | [0.41, 0.79]              |
| **GPT-4o-mini** | **02** | 0.26                 | [0.19, 0.34]              | 0.58                  | [0.48, 0.67]              |
| **LLaMA2**   | **03** | 0.28                 | [0.10, 0.38]              | 0.45                  | [0.22, 0.65]              |
| **Mistral**  | **03** | 0.42                 | [0.12, 0.71]              | 0.69                  | [0.28, 0.88]              |
| **LLaMA3**   | **03** | 0.33                 | [0.16, 0.50]              | 0.60                  | [0.37, 0.75]              |
| **GPT-4o-mini** | **03** | 0.25                 | [0.18, 0.33]              | 0.58                  | [0.47, 0.66]              |
| **LLaMA2**   | **04** | 0.35                 | [0.15, 0.54]              | 0.61                  | [0.35, 0.78]              |
| **Mistral**  | **04** | 0.55                 | [0.3, 0.76]               | 0.79                  | [0.56, 0.9]               |
| **LLaMA3**   | **04** | 0.45                 | [0.22, 0.62]              | 0.70                  | [0.47, 0.82]              |
| **GPT-4o-mini** | **04** | 0.25                 | [0.18, 0.33]              | 0.57                  | [0.46, 0.66]              |
