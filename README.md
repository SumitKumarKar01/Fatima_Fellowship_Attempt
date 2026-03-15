# Fatima_Fellowship_Attempt

This project analyzes quotes from a fictional inquiry on WiC through a complete NLP sentiment analysis workflow. The work includes cleaning the quotation text, translating Bangla content into English, generating positive, neutral, and negative sentiment scores with transformer models, visualizing sentiment patterns across groups and participants, and comparing NLP-based sentiment labels with human annotations to evaluate model alignment.

## Project Workflow

### Stage 1: Data Preparation and Sentiment Scoring
The project begins in `Sentiment_Analysis.ipynb`, where the raw Excel workbook is prepared for NLP processing.

- Loads the source Excel workbook with `pandas.read_excel()` and reads the Combined, M3, Oporajita, and Design sheets.
- Uses the `Description` column as the main text field for analysis.
- Cleans the text with a regular expression using Python `re` to remove participant suffixes such as `- P1`, `- P2`, and similar tags.
- Saves the cleaned data as an intermediate Excel file named `cleaned_dataset.xlsx`.
- Detects the language of each response with the `langdetect` library.
- Translates Bangla responses into English with the Hugging Face `pipeline()` translation model `csebuetnlp/banglat5_nmt_bn_en`.
- Stores the translated output in another Excel file named `translated_dataset.xlsx`.
- Runs sentiment analysis with the Hugging Face transformer model `cardiffnlp/twitter-roberta-base-sentiment-latest`.
- Produces three scores for each response: positive, neutral, and negative.
- Writes the final scored dataset to `calculated_sentiment_dataset.xlsx` for later visualization and comparison.

At the end of this stage, the repository has a cleaned, translated, and sentiment-scored dataset that can be reused in the later analysis notebooks.

### Stage 2: Visualization and Exploratory Analysis
The second stage is handled in `Graphing_Sentiment.ipynb`, where the sentiment outputs are converted into interpretable summaries and visual comparisons.

- Loads `calculated_dataset.xlsx` with `pandas` and separates the Combined, M3, Oporajita, and Design sheets into individual dataframes.
- Computes overall average sentiment scores with dataframe aggregation using `.agg()`.
- Visualizes overall sentiment with `matplotlib` bar charts and pie charts.
- Uses color-coded and hatched chart styling to distinguish positive, neutral, and negative sentiment visually.
- Groups the data by `Group` with `pandas.groupby()` to calculate location-wise or group-wise average sentiment.
- Displays group-level comparisons with stacked horizontal bar charts.
- Groups the data by `Participant` and calculates participant-level average sentiment scores.
- Uses a regex-based numeric extraction technique to sort participants in logical order instead of plain alphabetical order.
- Visualizes participant-level sentiment using stacked horizontal charts across M3, Oporajita, and Design.
- Compares M3 and Oporajita by converting the strongest sentiment score into class labels and then generating a confusion matrix.
- Uses `sklearn.metrics.confusion_matrix` and `seaborn.heatmap` to display agreement or mismatch patterns between datasets.
- Calculates summary statistics such as minimum, mean, and maximum sentiment scores for each sentiment type.
- Displays those summary results with grouped bar charts using `matplotlib` and tabular summaries using the `tabulate` library.

This stage turns the model outputs into project-level findings by showing sentiment trends overall, by group, and by participant.

### Stage 3: Human vs NLP Evaluation
The final stage is completed in `HumanVSNLP_Sentiment_Comp.ipynb`, where the model predictions are checked against human-labeled sentiment data.

- Loads the human-annotated and NLP-labeled sentiment workbook with `pandas.read_excel()`.
- Reads the required sheets, including M3 and Oporajita, and combines them with `pandas.concat()`.
- Removes incomplete rows by filtering out empty or missing `HumanSentiment` values.
- Builds a cleaned dataframe for comparison between `HumanSentiment` and `NLPSentiment`.
- Uses `pandas.crosstab()` with `normalize='index'` to create a percentage-based agreement table.
- Reorders the sentiment classes into `Positive`, `Neutral`, and `Negative` to keep the matrix consistent and readable.
- Visualizes the percentage agreement table with `seaborn.heatmap()`.
- Creates a second `pandas.crosstab()` without normalization to produce raw count comparisons.
- Visualizes the raw counts with another `seaborn` heatmap.
- Uses these two techniques, percentage cross-tabulation and count-based cross-tabulation, to evaluate how closely the NLP sentiment labels align with human annotation.

This final comparison stage helps measure how reliable the NLP-based sentiment classification is when compared with manual human judgment.

## Repository Structure

- `Sentiment_Analysis.ipynb` prepares the text data and generates sentiment scores.
- `Graphing_Sentiment.ipynb` analyzes and visualizes the scored outputs.
- `HumanVSNLP_Sentiment_Comp.ipynb` evaluates NLP sentiment against human annotation.
