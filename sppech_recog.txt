import speech_recognition as sr
import time 
import playsound
import os
import random
from gtts import gTTS
from time import ctime
import webbrowser

r=sr.Recognizer()
def record_myaudio(ask=False):
 with sr.Microphone() as source:
    if ask:
     jayanthi_speak(ask)
   
        
    audio=r.listen(source)
    voice_data=''
    try:
      voice_data=r.recognize_google(audio)
          

    except sr.UnknownValueError:
            jayanthi_speak ('SORRY, I DID NOT GET THAT')
    except sr.RequestError:
            jayanthi_speak('my service is down for now')
    return voice_data  




def jayanthi_speak(audio_string):
  tts=gTTS(text=audio_string, lang='en')
  r=random.randint(1,1000000)
  audio_file= 'audio-' + str(r) + '.mp3'
  tts.save(audio_file)
  playsound.playsound(audio_file)
  print (audio_string)
  os.remove(audio_file)




def respond(voice_data):
  if 'what is your name' in voice_data:
    jayanthi_speak ('My name is jayanthi')
  
  if 'what time is it' in voice_data:
    my_time='time in chennai is'
    jayanthi_speak(my_time.title()+' '+ctime().title())

  if 'search something for me' in voice_data:
   search = record_myaudio('what do you want to search for')
   url= 'http://google.com/search?q='+search
   webbrowser.get().open(url)
   jayanthi_speak ('here is what i found '.title()+search)

  if 'search location'  in voice_data:
    location=record_myaudio('name your location')
    url='http://google.nl/maps/place/'+location+'/&amp'
    webbrowser.get().open(url)
    jayanthi_speak ('here is the location '.title()+location)
  if 'exit' in voice_data :
    jayanthi_speak('thank you vishal')
    exit()

time.sleep(1)
jayanthi_speak('How can i help you,vishal')
while 1:
 voice_data=record_myaudio()
 respond(voice_data)
