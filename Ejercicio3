// Simulador de administración de memoria en C++
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

#define MEMORIA_TOTAL_MB 64
#define TAMANO_BLOQUE_KB 1024 // Cada bloque representa 1MB
#define BLOQUES_MEMORIA (MEMORIA_TOTAL_MB * 1024 / TAMANO_BLOQUE_KB)

struct Bloque {
    bool ocupado;
    int id_proceso;
};

class Memoria {
private:
    std::vector<Bloque> bloques;
public:
    Memoria() {
        bloques.resize(BLOQUES_MEMORIA, {false, -1});
    }

    bool asignar_proceso(int id, int tamanio_kb, bool dinamica = false) {
        int bloques_necesarios = (tamanio_kb + TAMANO_BLOQUE_KB - 1) / TAMANO_BLOQUE_KB;
        int inicio = -1;
        int libres = 0;

        for (int i = 0; i < BLOQUES_MEMORIA; ++i) {
            if (!bloques[i].ocupado) {
                if (inicio == -1) inicio = i;
                libres++;
                if (libres == bloques_necesarios) break;
            } else {
                inicio = -1;
                libres = 0;
            }
        }

        if (libres >= bloques_necesarios) {
            for (int i = inicio; i < inicio + bloques_necesarios; ++i) {
                bloques[i].ocupado = true;
                bloques[i].id_proceso = id;
            }
            std::cout << "Proceso " << id << " cargado.\n";
            return true;
        }

        std::cout << "Error: Memoria insuficiente para el proceso " << id << "\n";
        return false;
    }

    void liberar_proceso(int id) {
        bool encontrado = false;
        for (auto &b : bloques) {
            if (b.ocupado && b.id_proceso == id) {
                b.ocupado = false;
                b.id_proceso = -1;
                encontrado = true;
            }
        }
        if (encontrado) std::cout << "Proceso " << id << " liberado.\n";
        else std::cout << "Proceso " << id << " no encontrado.\n";
    }

    void compactar_memoria() {
        int index_libre = 0;
        for (int i = 0; i < BLOQUES_MEMORIA; ++i) {
            if (bloques[i].ocupado) {
                if (index_libre != i) {
                    bloques[index_libre] = bloques[i];
                    bloques[i].ocupado = false;
                    bloques[i].id_proceso = -1;
                }
                ++index_libre;
            }
        }
        std::cout << "Memoria compactada.\n";
    }

    void mostrar_estado() {
        std::cout << "\nEstado de la memoria:\n";
        for (int i = 0; i < BLOQUES_MEMORIA; ++i) {
            if (i % 64 == 0) std::cout << "\n";
            std::cout << (bloques[i].ocupado ? "#" : ".");
        }
        std::cout << "\n\n";
    }

    void calcular_fragmentacion() {
        int fragmentacion_interna = 0;
        int fragmentacion_externa = 0;
        int bloques_libres = 0;
        int huecos = 0;
        bool en_hueco = false;

        for (const auto &b : bloques) {
            if (!b.ocupado) {
                fragmentacion_externa++;
                if (!en_hueco) {
                    en_hueco = true;
                    ++huecos;
                }
            } else {
                en_hueco = false;
            }
        }

        std::cout << "Fragmentación interna estimada: 0 KB (modelo de bloques de 1MB)\n";
        std::cout << "Fragmentación externa: " << fragmentacion_externa << " MB en " << huecos << " huecos.\n";
    }
};

int main() {
    Memoria memoria;
    int opcion, id, tamanio;

    do {
        std::cout << "1. Cargar proceso\n2. Liberar proceso\n3. Compactar memoria\n4. Mostrar estado\n5. Ver fragmentación\n0. Salir\nOpción: ";
        std::cin >> opcion;
        switch (opcion) {
            case 1:
                std::cout << "ID del proceso: ";
                std::cin >> id;
                std::cout << "Tamaño en KB: ";
                std::cin >> tamanio;
                memoria.asignar_proceso(id, tamanio);
                break;
            case 2:
                std::cout << "ID del proceso a liberar: ";
                std::cin >> id;
                memoria.liberar_proceso(id);
                break;
            case 3:
                memoria.compactar_memoria();
                break;
            case 4:
                memoria.mostrar_estado();
                break;
            case 5:
                memoria.calcular_fragmentacion();
                break;
        }
    } while (opcion != 0);

    return 0;
}
