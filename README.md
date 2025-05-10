# CalcMedia
import tkinter as tk
from tkinter import messagebox
from PIL import ImageGrab

class MediaPonderadaApp:
    def __init__(self, root):
        self.root = root
        self.root.title("EduCalc")

        # Campos de Curso e Disciplina
        tk.Label(root, text="Curso:").grid(row=0, column=0, padx=5, pady=5)
        self.curso_entry = tk.Entry(root)
        self.curso_entry.grid(row=0, column=1, columnspan=2, padx=5, pady=5)

        tk.Label(root, text="Disciplina:").grid(row=1, column=0, padx=5, pady=5)
        self.disciplina_entry = tk.Entry(root)
        self.disciplina_entry.grid(row=1, column=1, columnspan=2, padx=5, pady=5)

        # Avaliações
        self.labels = []
        self.entradas_nota = []
        self.entradas_peso = []
        self.qtd_avaliacoes = 4

        for i in range(self.qtd_avaliacoes):
            label = tk.Label(root, text=f"Avaliação {i+1}")
            label.grid(row=i + 2, column=0, padx=5, pady=5)
            self.labels.append(label)

            entrada_nota = tk.Entry(root)
            entrada_nota.grid(row=i + 2, column=1, padx=5, pady=5)
            self.entradas_nota.append(entrada_nota)

            entrada_peso = tk.Entry(root)
            entrada_peso.grid(row=i + 2, column=2, padx=5, pady=5)
            self.entradas_peso.append(entrada_peso)

        # Botões
        self.botao_calcular = tk.Button(root, text="Calcular Média", command=self.calcular_media)
        self.botao_calcular.grid(row=self.qtd_avaliacoes + 2, column=0, columnspan=3, pady=10)

        self.resultado = tk.Label(root, text="", font=("Arial", 12, "bold"))
        self.resultado.grid(row=self.qtd_avaliacoes + 3, column=0, columnspan=3)

        self.icone_resultado = tk.Label(root, text="", font=("Arial", 20))
        self.icone_resultado.grid(row=self.qtd_avaliacoes + 4, column=0, columnspan=3)

        self.botao_salvar = tk.Button(root, text="Salvar como JPG", command=self.salvar_como_jpg)
        self.botao_salvar.grid(row=self.qtd_avaliacoes + 5, column=0, columnspan=3, pady=5)

    def calcular_media(self):
        try:
            soma_ponderada = 0
            total_pesos = 0
            for i in range(self.qtd_avaliacoes):
                nota = float(self.entradas_nota[i].get())
                peso = float(self.entradas_peso[i].get())
                soma_ponderada += nota * peso
                total_pesos += peso

            if total_pesos == 0:
                raise ZeroDivisionError

            media = soma_ponderada / total_pesos
            status = "Aprovado ✅" if media >= 6 else "Reprovado ❌"
            self.resultado.config(
                text=f"Curso: {self.curso_entry.get()} | Disciplina: {self.disciplina_entry.get()}\nMédia: {media:.2f} - {status}"
            )
            self.icone_resultado.config(text="✅" if media >= 6 else "❌")

        except ValueError:
            messagebox.showerror("Erro", "Insira apenas números válidos nas notas e pesos.")
        except ZeroDivisionError:
            messagebox.showerror("Erro", "A soma dos pesos não pode ser zero.")

    def salvar_como_jpg(self):
        # Pega coordenadas da janela
        x = self.root.winfo_rootx()
        y = self.root.winfo_rooty()
        w = x + self.root.winfo_width()
        h = y + self.root.winfo_height()
        # Captura imagem da tela
        imagem = ImageGrab.grab().crop((x, y, w, h))
        imagem.save("resultado_media.jpg")
        messagebox.showinfo("Imagem Salva", "A imagem foi salva como resultado_media.jpg")

# Executar interface
if __name__ == "__main__":
    root = tk.Tk()
    app = MediaPonderadaApp(root)
    root.mainloop()

