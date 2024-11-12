# translator
# Tradutor: Azure e OpenAI

## Azure

> Criar 2 resource groups:
>
>> Translator
>> Azure OpenAI

## Colab
### Gerar os códigos
```
!pip install requests python-docx
```
```
import requests
from docx import Document
import os 
subscription_key = 'key gerada noo azure'
endpoint = 'endpoint gerado no azure'
location = 'location gerado no azure'
target_language = 'pt.br'

def translate_text(text, target_language):
    path = '/translate'
    constructed_url = endpoint + path
    headers = {
        'Azure.subscription_key': subscription_key,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(os.urandom(16))
    }

    body = [{
        'text': text
    }]

   params = {
       'api-version' = '3.0',
       'from': 'en',
       'to': [target_language]
   }

   response = requests.post(constructed_url, headers=headers, json=body, params=params)
   response = response.json()
   return response[0]['translations'][0]['text']
   }
```
```
translate_text("When I was your man", target_language)
```
Upload de documento
```
def translate_document(path):
  document = Document(path)
  full_text = []
  for paragraph in document.paragraphs:
    translated_text = translate_text(paragraph.text, target_language),
    full_text.append(translated_text)

    translated_doc = Document()
    for line in full_text:
      translated_doc.add_paragraph(line)
    path_translated = path.replace('.docx', f'_{target_language}translated.docx')
    translated_doc.save(path_translated)
    return path_translated
```
```
input_file = "/content/Bruninho_Marcio.docx"
translate_document(input_file)
```

## Tradução de artigo

*Código - tradução de URL*

```
!pip install requests beautifulsoup4 openai langchain-openai
```
## Link

site com modelos de arquivos [Dev.to](https://dev.to/bhagvank/open-ai-codex-react-18imhttps://markdownlivepreview.com/).
```
import requests 
from bs4 import BeautifulSoup

def extract_text_from_url(url):
  response = requests.get(url)
  soup = BeautifulSoup(response.text, 'html.parser')
  text = soup.get_text()
  return text

extract_text_from_url("https://dev.to/bhagvank/open-ai-codex-react-18im")
```
_Limpar texto_
```
import requests 
from bs4 import BeautifulSoup

def extract_text_from_url(url):
  response = requests.get(url)

  if response.status_code == 200:
    soup = BeautifulSoup(response.text, 'html.parser')
    for script_or_style in soup(['script', 'style']):
      script_or_style.decompose()
    texto = soup.get_text(separator= ' ')
    linhas = (line.strip() for line in texto.splitlines())
    parts = (phrase.strip() for line in linhas for phrase in line.split(" "))
    texto_limpo = '\n'.join(parts)
    
    if texto:
     return texto_limpo
    
    else: 
      print(f"Failed to fetch URL. Status code: {response.status_code}")
    return None

  text = soup.get_text()
  return text


extract_text_from_url("https://dev.to/bhagvank/open-ai-codex-react-18im")
```
*Usando langchain: traduzir parte do artigo*
```
from langchain_openai.chat_models import AzureChatOpenAI

client = AzureChatOpenAI(
    azure_endpoint = "colar endpoint do azure"
    api_key = "colar api_key do azure"
    deployment_name = "gpt-4o-mini"
    max_retries = 0
)

def translate_text(text, target_language):
  messages = [
      ("system", "Você é um tradutor de textos"),
      ("user", f"Traduza o {text} para o idioma {lang} e responda em markdown")
      ]

  response = client.invoke(messages)
  print(response.content)
  return response.content 

  translate_article("You can access the application at", "português")
  ```
 *Traduzir artigo a partir da URL*
 ```
 url = "https://dev.to/bhagvank/open-ai-codex-react-18im"
text = extract_text_from_url(url)
# Changed translate_article to translate_text
article = translate_text(text, "pt-br")  
print(article)
```


### Espero que este passo a passo seja útil!


##

![This is an alt text.](https://www.gstatic.com/webp/gallery/4.webp "This is a sample image.")


