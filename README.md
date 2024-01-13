import pyttsx3
import speech_recognition as sr
import datetime
import pyjokes
import pywhatkit
import pyautogui
import wikipedia
import os
import webbrowser

listner = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)
engine.setProperty('rate', 250)

def talk(text):
    engine.say(text)
    engine.runAndWait()

def take_command():
    try:
        with sr.Microphone() as source:
            print('listening...')
            voice = listner.listen(source)
            print('listening done...')
            command = listner.recognize_google(voice)
            command = command.lower()
            print('Command recognized:', command)  
            return command
    except Exception as e:
        print(f"Error: {e}")
        return ""


def greeting():
    current_time=datetime.datetime.now()
    hour=current_time.hour
    if 3 <= hour<12:
        talk('good moring sir!')
    elif 12<= hour <18:
        talk('good afternoon sir!')
    else:
        talk('good evening sir!')
        

def run_jarvis():
    command = take_command()
    print("Command received:", command)
    if 'hello' in command or 'whats up' in command:
        talk('hi boss how are you')
        
    elif 'joke' in command:
        talk(pyjokes.get_joke())
        
    elif 'exit' in command:
        talk('goodbye!')
        exit()
        
    elif 'play' in command:
        song=command.replace('play',"")
        talk('playing'+ command)
        pywhatkit.playonyt(song)
        
    elif 'time' in command:
        time= datetime.datetime.now().strftime("%I:%M %p")
        print(time)
        talk("Current time is"+time)
     
    elif 'open' in command:
        command=command.replace('open',"")
        pyautogui.press('super')
        pyautogui.typewrite(command)
        pyautogui.sleep(1)
        pyautogui.press('enter')
        talk('opening'+ command)       
        
    elif 'close' in command:
        talk('closing sir!')
        pyautogui.hotkey('alt','f4')
        
    elif 'who is' in command or 'what is' in command:
        person=command.replace('who is',"")
        info=wikipedia.summary(person,2)
        print(info)
        talk(info)
           
    elif 'remember that' in command:
        rememberMessage=command.replace('remember that',"")
        talk('you told me to remember that'+ rememberMessage)
        remember=open('remember.txt',"a")
        remember.write(rememberMessage)
        remember.close 
              
    elif 'what do you remember' in command:
         remember=open('remember.txt','r')    
         talk('you told me to remember'+ remember.read())
     
    elif 'clear remember file' in command:
        file=open('remember.txt','w')
        file.write(f"")
        talk('done sir! everything i remember is deleted')
        
    elif 'shutdown' in command:
        talk('closing the pc in')
        talk('3.2.1')
        os.system("shutdown /s /t 1")
        
        
    elif 'restart' in command:
        talk('restarting the pc in')
        talk('3.2.1')
        os.system("shutdown /r /t 1")
    
    elif 'search' in command:
        usercm=command.replace('search',"")
        usercm=usercm.lower()
        webbrowser.open(f"https://www.google.com/search?q={usercm}")
        talk('this is what i found on the internet.')
        
    elif "stop jarvis" in command or 'start jarvis' in command:
        pyautogui.press('k')
        talk('done sir!')

    elif "full screen" in command:
        pyautogui.press('f')
        talk('done sir!')
        
    else:
      talk('I dont understand')

greeting()
talk('hello world my name is jarvisx')
while True:
    run_jarvis()
