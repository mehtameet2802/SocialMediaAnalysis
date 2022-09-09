!pip install transformers
!pip install ekphrasis
from transformers import AutoModelForSequenceClassification
from transformers import TFAutoModelForSequenceClassification
from transformers import AutoTokenizer
import numpy as np
from scipy.special import softmax
import csv
import urllib.request
import pandas as pd
import plotly
from plotly import graph_objects as go
import plotly.express as px
import ekphrasis
from ekphrasis.classes.preprocessor import TextPreProcessor
from ekphrasis.classes.tokenizer import SocialTokenizer
from ekphrasis.dicts.emoticons import emoticons




# Define the model:

def preprocess(text):
    new_text = []
    for t in text.split(" "):
        t = '@user' if t.startswith('@') and len(t) > 1 else t
        t = 'http' if t.startswith('http') else t
        new_text.append(t)
    return " ".join(new_text)
    

# Tasks:
# emoji, emotion, hate, irony, offensive, sentiment
task='sentiment'
MODEL = f"cardiffnlp/twitter-roberta-base-{task}"
tokenizer = AutoTokenizer.from_pretrained(MODEL)
# download label mapping
labels=[]
mapping_link = f"https://raw.githubusercontent.com/cardiffnlp/tweeteval/main/datasets/{task}/mapping.txt"
with urllib.request.urlopen(mapping_link) as f:
    html = f.read().decode('utf-8').split("\n")
    csvreader = csv.reader(html, delimiter='\t')
labels = [row[1] for row in csvreader if len(row) > 1]
# PT
model = AutoModelForSequenceClassification.from_pretrained(MODEL)
model.save_pretrained(MODEL)

df = pd.read_csv(<selection> + '.csv')            # where <selection> is one of the .csv files produced by the Reddit or Twitter data collection.


# Define a function that takes in text and the model as inputs and returns a negative or positive sentiment: 

def analyze(text, model, no_neutral=False, raw = False):
  processed_text = preprocess(text)
  encoded_input = tokenizer(processed_text, return_tensors='pt')
  output = model(**encoded_input)
  scores = output[0][0].detach().numpy()
  scores = softmax(scores)
  if raw==True:
    return(scores)
  if no_neutral==True:
    scores[1] = 0
  return np.argmax(scores)                        # The output is actually either 0 or 2, but we map those to strings "negative" and "positive" later.
  





# For Twitter, where the .csv files are already seperated by categories:
outputs=[]                        
labels = {0: 'Negative', 1:'Neutral', 2:'Positive'}
for i in tqdm(df['Processed Text']):
  res = analyze(i, model, raw=False, no_neutral=True)
  outputs.append([i,res])
  
  
  
  
  
  
# For Reddit, where we need to add a bit more information to the table.
new_comments=[]
for comment in tqdm(comments):
  text = comment[3]
  if (not 'was automatically removed because' in text) and (not text == '[removed]') and (not text == '[deleted]') and len(text) < 500:
    output_array = []

    text = process(text)

    output_array.append(text)

    article = comment[1]
    for i in list(categories.keys()):
      for i2 in categories[i]:
        if i2 in article:
          site = i2
    output_array.append(site)

    output_array.append(eval(comment[2]))

    new_comments.append(output_array)
 
 # The outputs of the above code are arrays in the form [<processed text>, <site>, <partisanship>, <sentiment>]
 
 

 # From this, we obtained all the information we used in our study about the sentiment of posts and comments.
