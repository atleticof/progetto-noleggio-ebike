# progetto-noleggio-ebike
Questo codice simula un app di noleggio di e-bike, tenendo conto di possibili ritardi, autonomia e disponibilità delle e-bike rendendolo una simulazione dettagliata.

#include <iostream>
#include <ctime>
#include <cstdlib>
#include <cmath>
#include <string>
#define N 20

using namespace std;

struct ebike {
    string id;
    int asseX;
    int asseY;
    int d;
    int autonomia;
    bool disponibile;
};

void ordinamentoD(ebike lista[], int n = N) {
    if (n <= 1) return;

    for (int i = 0; i < n - 1; i++) {
        if (lista[i].d > lista[i + 1].d) {
            ebike temp = lista[i];
            lista[i] = lista[i + 1];
            lista[i + 1] = temp;
        }
    }

    ordinamentoD(lista, n - 1);
}

int main() {
    srand(time(0));

    ebike lista[N];

    for (int i = 0; i < N; i++) {
        lista[i].id = "E-bike" + to_string(i + 1);
        lista[i].asseX = rand() % 500;
        lista[i].asseY = rand() % 500;
        lista[i].autonomia = rand() % 30;
        lista[i].disponibile = rand() % 2;
    }

    int utenteX, utenteY;

    cout << "inserisci le tue coordinate attuali: " << endl;
    cin >> utenteX;
    cout << endl;
    cin >> utenteY;

    for (int i = 0; i < N; i++) {
        lista[i].d = sqrt(pow(lista[i].asseX - utenteX, 2) + pow(lista[i].asseY - utenteY, 2));
    }

    ordinamentoD(lista);

    cout << "\nEbike piu' vicine:" << endl;
    for(int i = 0; i < N; i++) {
        cout << lista[i].id << " - Distanza: " << lista[i].d << " metri - autonomia: " << lista[i].autonomia << " kilometri - Stato: " << (lista[i].disponibile ? "Disponibile" : "Non disponibile") << endl;
    }

    string ebikeSelezionata;
    int c = 0, contatore = 0, kmUtente;
    int ritardo_casuale = rand() % 15 + 1;

    cout << "\nseleziona una bici: " << endl;
    cin >> ebikeSelezionata;

    while(c == 0){
        if(contatore >= 20){
            cout << "id inesistente" << endl;
            return 0;
        }
        if(lista[contatore].id == ebikeSelezionata){
            c = 1;
        }
        else{
            contatore += 1;
        }
    }

    if (!lista[contatore].disponibile) {
        cout << "L'e-bike selezionata non e' disponibile al momento." << endl;
        return 0;
    }

    cout << "quanti kilometri intendi percorrere?" << endl;
    cin >> kmUtente;

    if(kmUtente > lista[contatore].autonomia){
        cout << "autonomia e-bike insufficiente" << endl;
    }
    else {
        int tempo = ((float(kmUtente) / 20.0) * 60.0) + ritardo_casuale;
        cout << "minuti totali stimati: " << tempo << " minuti" << endl;
        cout << "km percorsi: " << kmUtente << " kilometri" << endl;
        cout << "costo finale: " << 0.20f * tempo << " euro" << endl;
    }

}
