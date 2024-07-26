# regex
import re
import spacy
from spacy.matcher import Matcher

nlp = spacy.load('en_core_web_sm')

texting = ("John Doe is a software engineer who lives in San-Francisco. You can contact him at john.doe@example.com or on his mobile phone at +1-415-555-1234. His colleague, Jane Smith, also works in the same company and can be reached at jane.smith@example.com or +1-415-555-5678. Another team member, Robert Brown, can be contacted via email at robert.brown@example.com or by phone at +1-415-555-6789. Sarah Johnson, the project manager, is available at sarah.johnson@example.com or +1-415-555-7890. For administrative support, reach out to Emily Davis at emily.davis@example.com or +1-415-555-8901. Lastly, Michael Wilson handles customer relations and can be reached at michael.wilson@example.com or +1-415-555-9012.")

def find_phone_numbers(text):
    phone_pattern = re.compile(r'\+\d{1,2}-\d{3}-\d{3}-\d{4}')
    return phone_pattern.findall(text)

def find_email_adress(text):
  mail_pattern=re.compile(r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}')
  return mail_pattern.findall(text)

def find_names(text):
    doc = nlp(text)
    names = []
    
    pattern = [{'POS': 'PROPN'}, {'POS': 'PROPN'}]
    matcher = Matcher(nlp.vocab)
    matcher.add('NAME-SURNAME', [pattern])
    matches = matcher(doc)
    
    for match_id, start, end in matches:
        span = doc[start:end]
        names.append(span.text)
        
    return names

phone_numbers = find_phone_numbers(texting)
names = find_names(texting)
mails=find_email_adress(texting)

for i in range(len(names)):
    print(f"Name: {names[i]} --- Phone Number: {phone_numbers[i]} --- Mail Adress: {mails[i]}")
