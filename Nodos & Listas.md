- 👋 Hi, I’m @Karuky00
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

#include <iostream>
#include <locale>

using namespace std;

// Definición de la clase NodoLista
class NodoLista {
public:
    int numero;
    NodoLista* izquierda;
    NodoLista* derecha;

    NodoLista() : numero(0), izquierda(nullptr), derecha(nullptr) {}
};

// Definición de la clase Lista
class Lista {
private:
    NodoLista* primero;
    NodoLista* ultimo;

public:
    Lista() : primero(nullptr), ultimo(nullptr) {}

    void insertar(int num);
    void imprimirListaIzq();
    void imprimirListaDer();
};

void Lista::insertar(int num) {
    NodoLista* q = new NodoLista();
    q->numero = num;
    q->izquierda = nullptr;
    q->derecha = nullptr;

    if (!primero) {
        primero = q;
        ultimo = q;
    } else {
        ultimo->derecha = q;
        q->izquierda = ultimo;
        ultimo = q;
    }
}

void Lista::imprimirListaIzq() {
    NodoLista* temp = primero;
    while (temp) {
        cout << temp->numero << " ";
        temp = temp->derecha;
    }
    cout << endl;
}

void Lista::imprimirListaDer() {
    NodoLista* temp = ultimo;
    while (temp) {
        cout << temp->numero << " ";
        temp = temp->izquierda;
    }
    cout << endl;
}

int main() {
    setlocale(LC_CTYPE, "spanish");

    Lista lista;
    int opcion;

    do {
        cout << "\nElige la opción deseada:" << endl;
        cout << "[1] Insertar un nuevo dato" << endl;
        cout << "[2] Mostrar los datos de Izq -> Der" << endl;
        cout << "[3] Mostrar los datos de Der -> Izq" << endl;
        cout << "[0] Salir" << endl;
        cin >> opcion;

        switch (opcion) {
            case 1:
                int num;
                cout << "Ingrese el dato: ";
                cin >> num;
                lista.insertar(num);
                break;
            case 2:
                lista.imprimirListaIzq();
                break;
            case 3:
                lista.imprimirListaDer();
                break;
            default:
                cout << "¡Opción incorrecta!" << endl;
        }
    } while (opcion != 0);

    return 0;
}
