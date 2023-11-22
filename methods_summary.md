## Resources Used:
The code utilizes various resources in its workflow:

**PubMed Central (PMC):** PMC is used to search for and retrieve scientific articles related to drug interactions with the BCRP (Breast Cancer Resistance Protein) transporter. The articles serve as the primary source of information.

**PubChem:** PubChem is used to fetch Canonical SMILES (Simplified Molecular Input Line Entry System) and Chemical IDs (ChemID) for drug names mentioned in the articles. This information is crucial for identifying and categorizing drugs.

**BERT Model:** The code employs a pre-trained BERT (Bidirectional Encoder Representations from Transformers) model for text classification. It predicts whether a drug is a substrate or an inhibitor of the BCRP transporter based on the drug's context within the articles.

**Bioinformatics Libraries:** Libraries such as Biopython (for PubMed Central access) and BeautifulSoup (for HTML parsing) are used for data retrieval and preprocessing.

## Method of Document Searching and Collection:
Since the goal is to find only relative articles, the code begins by searching PubMed Central (PMC) with specific queries related to BCRP transporter interactions. By queries mean we pass a list of keywords(more keywords means more articles) for ex: ["BCRP Drug Transport", "BCRP Multidrug Resistance"]. Our code then uses BioPython package to fetch PMC IDs of all the articles that contain these keywords in their metadata such as Title, Abstract etc. It then proceeds to fetch and process each article to extract drug names and their contexts within the articles. Pubmed recommends bulk fetching of data between 9PM and 5AM on weekdays or on weekends(basically outside of peak hours). Though, we have applied time delays in our code to adhere with recommended requests limit per second to avoid getting our IP address blocked by Pubchem and Pubmed for this challenge.

## Workflow for Extracting and Labeling Drugs:
**Article Retrieval:** The code retrieves articles based on search queries from PMC.

**Article Processing:** Each article's HTML content is fetched and processed. The article's title is extracted, and the article text is cleaned and tokenized.

**Drug Name Extraction:** The code identifies drug names within the text using a drug named entity recognition method. It creates a list of drug names and their associated context within the article.

**Labeling as Substrate or Inhibitor:** For each drug extracted, the code predicts whether it is a substrate or an inhibitor of the BCRP transporter using the pre-trained BERT model. We trained the BERT model on data(drug names and their context in which they are used) that we curated manually from research articles and labelling them as substrate/inhibitors.

We also applied various techniques to fine-tune our model such as SMOTE for classes imbalance, K-cross validation etc.

**Canonical SMILES and ChemID Retrieval:** For each drug, the code fetches its Canonical SMILES and ChemID from PubChem, storing them in a cache to avoid redundant requests.
API Used: "https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/<drug_name>/property/CanonicalSMILES/JSON"

**Data Aggregation:** The code aggregates the drug information, including PMC IDs, Canonical SMILES, and ChemID, into a DataFrame.

## Checking Results for Validity:
To check the validity of the results, the following steps are taken:

**Matching Predictions:** The code compares its predictions (substrate or inhibitor) with the actual labels from training sample data provided by Lantern Pharma team for drugs with matching names. It calculates the accuracy based on these matches.

**Metrics Summary:** The code generates a metrics summary that includes the team's name, contact information, the number of samples processed, the number of references retrieved, the number of inhibitors and substrates predicted, the percentage of minority class samples, the number of drugs with available Canonical SMILES, and the percentage of drugs with SMILES available.

## Adapting Code to Different Proteins or Topics:
Our code is like a chameleon, easily adaptable to study other proteins or dive into different topics. All it takes is tweaking the search queries for a new protein or topic, and giving our BERT model a fresh set of data to learn from. Whole workflow is fully automated and can be scheduled to run at any desired time without manual intervention.

We've crafted our code with flexibility in mind, dividing it into functions that can be tweaked for various scenarios:

**search_pubmed_central():** This is your key to unlocking articles on any topic. Just throw in some keywords and it'll fetch the relevant PMC IDs.

**fetch_and_process_article():** Give it a PMC ID, and it’ll get you the full text, pinpoint drug names, and attach those all-important SMILES and ChemIDs. We’ve even included a keyword filter to ensure we’re only picking up drugs relevant to our topic.

**get_canonical_smiles_and_chem_id():** Need a SMILE or a ChemID for a drug? This function’s got you covered.

So, there you have it! For detailed information please check out the code.
