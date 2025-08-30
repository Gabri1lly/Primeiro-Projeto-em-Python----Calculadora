# Calculadora por Voz 🧠

Este projeto é uma calculadora simples com interface gráfica feita em Python que permite calcular expressões matemáticas digitadas ou faladas. Utiliza reconhecimento de voz para captar a expressão e síntese de voz para falar o resultado.

---

## Funcionalidades

- Entrada de expressões matemáticas via teclado ou voz.
- Reconhecimento de voz em português brasileiro.
- Cálculo seguro das expressões usando funções matemáticas do módulo `math`.
- Resposta falada com o resultado do cálculo.
- Interface gráfica simples e intuitiva com Tkinter.

---

## Tecnologias utilizadas

- Python 3
- Tkinter (interface gráfica)
- SpeechRecognition (reconhecimento de voz)
- Pyttsx3 (síntese de voz)
- math (funções matemáticas)

---

## Como usar

1. Clone este repositório:
   ```
   git clone https://github.com/seu-usuario/calculadora-voz.git
   cd calculadora-voz
   ```

2. Instale as dependências:
   ```
   pip install SpeechRecognition pyttsx3
   ```

3. Execute o script:
   ```
   python nome_do_arquivo.py
   ```

4. Na janela que abrir:
   - Digite uma expressão matemática e clique em **Calcular** para ver o resultado.
   - Ou clique em **Falar** e diga a expressão para que ela seja reconhecida, calculada e o resultado falado.

---

## Exemplo de expressões válidas

- `2 + 2`
- `sqrt(16)`
- `sin(pi / 2)`
- `abs(-5)`
- `round(3.1415, 2)`

---

## Observações

- O reconhecimento de voz depende do microfone e da conexão com a internet (Google Speech API).
- O cálculo é feito de forma segura, permitindo apenas funções matemáticas definidas no código.
- Caso a expressão seja inválida ou não compreendida, uma mensagem de erro será exibida e falada.

---

## Código principal

```python
from tkinter import *
import speech_recognition as sr
import pyttsx3
import math

# Setup do conversor de texto para fala
voz = pyttsx3.init()
voz.setProperty('rate', 170)

# Cria um dicionário seguro com todas as funções de math
safe_dict = {k: getattr(math, k) for k in dir(math) if not k.startswith("__")}
safe_dict.update({'abs': abs, 'round': round})

def falar(texto):
    voz.say(texto)
    voz.runAndWait()

def calcular(expressao):
    try:
        resultado = eval(expressao, {"__builtins__": None}, safe_dict)
        return str(resultado)
    except Exception as e:
        return "Erro"

def ouvir():
    r = sr.Recognizer()
    with sr.Microphone() as fonte:
        entrada.delete(0, END)
        entrada.insert(0, "🎤 Ouvindo...")
        app.update()
        try:
            audio = r.listen(fonte, timeout=5)
            frase = r.recognize_google(audio, language="pt-BR")
            entrada.delete(0, END)
            entrada.insert(0, frase)
            resultado = calcular(frase)
            resultado_label.config(text=f"Resultado: {resultado}")
            falar(f"O resultado é {resultado}")
        except:
            entrada.delete(0, END)
            resultado_label.config(text="Não entendi")
            falar("Não entendi o que você disse")

def calcular_botao():
    expressao = entrada.get()
    resultado = calcular(expressao)
    resultado_label.config(text=f"Resultado: {resultado}")
    if resultado != "Erro":
        falar(f"O resultado é {resultado}")
    else:
        falar("Erro no cálculo")

# Interface
app = Tk()
app.title("🧠 Calculadora por Voz")
app.geometry("400x250")
app.configure(bg="#e9f5ff")

Label(app, text="Digite ou fale a expressão:", bg="#e9f5ff", font=("Arial", 12)).pack(pady=10)

entrada = Entry(app, font=("Arial", 14), width=30, justify="center")
entrada.pack(pady=5)

botao_calcular = Button(app, text="🧮 Calcular", command=calcular_botao, width=15)
botao_calcular.pack(pady=5)

botao_voz = Button(app, text="🎙️ Falar", command=ouvir, width=15, bg="#d1f7d6")
botao_voz.pack(pady=5)

resultado_label = Label(app, text="Resultado: ", bg="#e9f5ff", font=("Arial", 12, "bold"))
resultado_label.pack(pady=15)

app.mainloop()
```

---

## Licença

Este projeto está sob a licença MIT. Sinta-se à vontade para usar e modificar.

---

Se tiver dúvidas ou sugestões, abra uma issue!
