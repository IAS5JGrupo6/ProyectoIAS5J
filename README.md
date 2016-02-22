# ProyectoIAS5J
Debe descargar el diccionario.txt 
Mayor informacion escribir al siguiente correo:
janzules23@gmail.com
telefono: 2228258-0993522705

import re, collections
from re import UNICODE 
def words(text): 
    return re.findall('[a-záéíóúñ]+', text.lower(), UNICODE)
  
def train(features):  
    model = collections.defaultdict(lambda: 1)  
    for f in features:  
        model[f] += 1  
    return model  
f=open("diccionario.txt")  
NWORDS = train(words(f.read()))  
alphabet = 'abcdefghijklmnopqrstuvwxyzáéíóúñ'  
  
def edits1(word):  
   s = [(word[:i], word[i:]) for i in range(len(word) + 1)]  
   deletes    = [a + b[1:] for a, b in s if b]  
   transposes = [a + b[1] + b[0] + b[2:] for a, b in s if len(b)>1]  
   replaces   = [a + c + b[1:] for a, b in s for c in alphabet if b]  
   inserts    = [a + c + b     for a, b in s for c in alphabet]  
   return set(deletes + transposes + replaces + inserts)  
  
def known_edits2(word):  
    return set(e2 for e1 in edits1(word) for e2 in edits1(e1) if e2 in NWORDS)  
  
def known(words):
     return set(w for w in words if w in NWORDS)  
  
def correct(word):  
    candidates = known([word]) or known(edits1(word)) or known_edits2(word) or [word]  
    return max(candidates, key=NWORDS.get)  


print ("\t\t**********************************")
print ("\t\t* Bienvenidos a Nuestro Proyecto *")
print ("\t\t**********************************")

texto=input("\nPalabra a corregir: ")
print ("\nLa palabra corregida es:\n\t\t"+correct(texto)+"\n\n")
