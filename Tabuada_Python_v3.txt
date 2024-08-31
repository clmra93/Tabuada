import tkinter as tk
from tkinter import messagebox
import random

# Inicializar contadores de acertos e erros
acertos = 0
erros = 0

# Inicializar a lista de perguntas feitas
perguntas_feitas = set()

# Função para gerar uma nova pergunta
def gerar_pergunta():
    global num1, num2, resposta_correta, perguntas_feitas
    
    if len(perguntas_feitas) >= 50:
        exibir_resultados()
        return
    
    while True:
        num1 = random.randint(1, 10)
        num2 = random.randint(1, 10)
        if (num1, num2) not in perguntas_feitas:
            perguntas_feitas.add((num1, num2))
            break
    
    resposta_correta = num1 * num2
    pergunta.config(text=f"{num1} x {num2} = ?")
    resposta_entry.delete(0, tk.END)

# Função para verificar a resposta
def verificar_resposta():
    global acertos, erros
    try:
        resposta_usuario = int(resposta_entry.get())
        if resposta_usuario == resposta_correta:
            messagebox.showinfo("Resultado", "Parabéns, você acertou!")
            acertos += 1
        else:
            messagebox.showinfo("Resultado", f"Que pena, você errou. O resultado é {resposta_correta}")
            erros += 1
        gerar_pergunta()
    except ValueError:
        messagebox.showerror("Erro", "Por favor, insira um número válido.")
        resposta_entry.delete(0, tk.END)

# Função para exibir o resultado final
def exibir_resultados():
    messagebox.showinfo("Resultados Finais", f"Acertos: {acertos}\nErros: {erros}")
    root.quit()

# Função para exibir a mensagem de boas-vindas
def comeco():
    messagebox.showinfo("Bem-vindo", "Bem-vindo à Tabuada! Você receberá algumas multiplicações para resolver e treinar seu aprendizado. Boa sorte!")
    gerar_pergunta()

# Configurar a janela principal
root = tk.Tk()
root.title("Tabuada Aleatória")

pergunta = tk.Label(root, text="", font=("Helvetica", 16))
pergunta.pack(pady=20)

resposta_entry = tk.Entry(root, font=("Helvetica", 16))
resposta_entry.pack(pady=10)

verificar_button = tk.Button(root, text="Verificar Resposta", command=verificar_resposta, font=("Helvetica", 16))
verificar_button.pack(pady=20)

finalizar_button = tk.Button(root, text="Finalizar e Mostrar Resultados", command=exibir_resultados, font=("Helvetica", 16))
finalizar_button.pack(pady=10)

# Exibir a mensagem de boas-vindas e gerar a primeira pergunta
comeco()

# Iniciar o loop principal do tkinter
root.mainloop()
