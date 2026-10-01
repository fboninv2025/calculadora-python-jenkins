import tkinter as tk
from calculadora import sumar, restar, multiplicar, dividir


class CalculadoraApp:
    def __init__(self, ventana):
        self.ventana = ventana
        self.ventana.title("Calculadora")
        self.ventana.geometry("340x500")
        self.ventana.resizable(False, False)

        self.valor_actual = ""
        self.primer_numero = None
        self.operacion = None
        self.reiniciar_pantalla = False

        self.crear_interfaz()

    def crear_interfaz(self):
        # Título
        titulo = tk.Label(
            self.ventana,
            text="Calculadora",
            font=("Segoe UI", 16, "bold"),
            anchor="w"
        )
        titulo.pack(fill="x", padx=15, pady=(15, 5))

        # Pantalla
        self.pantalla = tk.Label(
            self.ventana,
            text="0",
            font=("Segoe UI", 36),
            anchor="e",
            padx=15,
            bg="white",
            relief="solid",
            bd=1
        )
        self.pantalla.pack(
            fill="x",
            padx=10,
            pady=(10, 15),
            ipady=15
        )

        # Contenedor de botones
        botones_frame = tk.Frame(self.ventana)
        botones_frame.pack(
            fill="both",
            expand=True,
            padx=8,
            pady=5
        )

        botones = [
            ("C", 0, 0, self.limpiar),
            ("⌫", 0, 1, self.borrar),
            ("+/-", 0, 2, self.cambiar_signo),
            ("÷", 0, 3, lambda: self.seleccionar_operacion("÷")),

            ("7", 1, 0, lambda: self.agregar_numero("7")),
            ("8", 1, 1, lambda: self.agregar_numero("8")),
            ("9", 1, 2, lambda: self.agregar_numero("9")),
            ("×", 1, 3, lambda: self.seleccionar_operacion("×")),

            ("4", 2, 0, lambda: self.agregar_numero("4")),
            ("5", 2, 1, lambda: self.agregar_numero("5")),
            ("6", 2, 2, lambda: self.agregar_numero("6")),
            ("−", 2, 3, lambda: self.seleccionar_operacion("−")),

            ("1", 3, 0, lambda: self.agregar_numero("1")),
            ("2", 3, 1, lambda: self.agregar_numero("2")),
            ("3", 3, 2, lambda: self.agregar_numero("3")),
            ("+", 3, 3, lambda: self.seleccionar_operacion("+")),

            ("0", 4, 0, lambda: self.agregar_numero("0")),
            (".", 4, 1, self.agregar_decimal),
            ("=", 4, 2, self.calcular_resultado),
        ]

        for texto, fila, columna, comando in botones:
            boton = tk.Button(
                botones_frame,
                text=texto,
                font=("Segoe UI", 16),
                command=comando
            )

            if texto == "0":
                boton.grid(
                    row=fila,
                    column=columna,
                    columnspan=2,
                    sticky="nsew",
                    padx=3,
                    pady=3
                )
            elif texto == ".":
                # El 0 ocupa las columnas 0 y 1.
                continue
            elif texto == "=":
                boton.grid(
                    row=fila,
                    column=2,
                    columnspan=2,
                    sticky="nsew",
                    padx=3,
                    pady=3
                )
            else:
                boton.grid(
                    row=fila,
                    column=columna,
                    sticky="nsew",
                    padx=3,
                    pady=3
                )

        # Colocamos decimal sobre el botón 0 extendido.
        # Para mantener una distribución más convencional:
        boton_decimal = tk.Button(
            botones_frame,
            text=".",
            font=("Segoe UI", 16),
            command=self.agregar_decimal
        )
        boton_decimal.grid(
            row=4,
            column=1,
            sticky="nsew",
            padx=3,
            pady=3
        )

        # Ajustamos 0 a una sola columna
        for widget in botones_frame.grid_slaves(row=4, column=0):
            widget.grid_configure(columnspan=1)

        for i in range(5):
            botones_frame.rowconfigure(i, weight=1)

        for i in range(4):
            botones_frame.columnconfigure(i, weight=1)

    def actualizar_pantalla(self):
        self.pantalla.config(
            text=self.valor_actual if self.valor_actual else "0"
        )

    def agregar_numero(self, numero):
        if self.reiniciar_pantalla:
            self.valor_actual = ""
            self.reiniciar_pantalla = False

        if self.valor_actual == "0":
            self.valor_actual = numero
        else:
            self.valor_actual += numero

        self.actualizar_pantalla()

    def agregar_decimal(self):
        if self.reiniciar_pantalla:
            self.valor_actual = ""
            self.reiniciar_pantalla = False

        if "." not in self.valor_actual:
            if not self.valor_actual:
                self.valor_actual = "0"

            self.valor_actual += "."

        self.actualizar_pantalla()

    def seleccionar_operacion(self, operacion):
        if not self.valor_actual:
            return

        self.primer_numero = float(self.valor_actual)
        self.operacion = operacion
        self.reiniciar_pantalla = True

    def calcular_resultado(self):
        if (
            self.primer_numero is None
            or self.operacion is None
            or not self.valor_actual
        ):
            return

        segundo_numero = float(self.valor_actual)

        try:
            if self.operacion == "+":
                resultado = sumar(
                    self.primer_numero,
                    segundo_numero
                )

            elif self.operacion == "−":
                resultado = restar(
                    self.primer_numero,
                    segundo_numero
                )

            elif self.operacion == "×":
                resultado = multiplicar(
                    self.primer_numero,
                    segundo_numero
                )

            elif self.operacion == "÷":
                resultado = dividir(
                    self.primer_numero,
                    segundo_numero
                )

            else:
                return

            if isinstance(resultado, float) and resultado.is_integer():
                resultado = int(resultado)

            self.valor_actual = str(resultado)
            self.actualizar_pantalla()

            self.primer_numero = None
            self.operacion = None
            self.reiniciar_pantalla = True

        except ValueError:
            self.pantalla.config(text="Error")
            self.valor_actual = ""
            self.primer_numero = None
            self.operacion = None
            self.reiniciar_pantalla = True

    def limpiar(self):
        self.valor_actual = ""
        self.primer_numero = None
        self.operacion = None
        self.reiniciar_pantalla = False
        self.actualizar_pantalla()

    def borrar(self):
        if self.reiniciar_pantalla:
            return

        self.valor_actual = self.valor_actual[:-1]
        self.actualizar_pantalla()

    def cambiar_signo(self):
        if not self.valor_actual:
            return

        if self.valor_actual.startswith("-"):
            self.valor_actual = self.valor_actual[1:]
        else:
            self.valor_actual = "-" + self.valor_actual

        self.actualizar_pantalla()


if __name__ == "__main__":
    ventana = tk.Tk()
    app = CalculadoraApp(ventana)
    ventana.mainloop()