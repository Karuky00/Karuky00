#include <windows.h>
#include <stdio.h> // Agregamos esta línea para usar printf

#define ID_LABEL 100

LRESULT CALLBACK ProcedimientoVentana(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam); // Corregimos el nombre de la función

int WINAPI WinMain(HINSTANCE hIns, HINSTANCE hInstanciaPrevia, LPSTR lpCmdLinea, int nCmdShow) {
    HWND ventana, hiconUser;
    HBITMAP hlogoBinario;
    MSG mensaje;
    WNDCLASSEX clase;

    clase.cbSize = sizeof(WNDCLASSEX);
    clase.cbClsExtra = 0;
    clase.style = CS_HREDRAW | CS_VREDRAW;
    clase.lpfnWndProc = ProcedimientoVentana; // Corregimos el nombre de la función
    clase.hInstance = hIns;
    clase.hIcon = LoadIcon(NULL, IDI_APPLICATION); // Cambiamos NULL por IDI_APPLICATION
    clase.hIconSm = LoadIcon(NULL, IDI_INFORMATION); // Cambiamos NULL por IDI_INFORMATION
    clase.hCursor = LoadCursor(NULL, IDC_ARROW);
    clase.lpszClassName = L"IdentificadorVentana"; // Agregamos 'L' para indicar cadena ampliada
    clase.hbrBackground = CreateSolidBrush(RGB(21, 232, 189));

    if (!RegisterClassEx(&clase)) {
        MessageBox(NULL, "No se pudo ejecutar la aplicacion", "Error", MB_ICONERROR);
        return EXIT_FAILURE;
    }

    ventana = CreateWindowEx(0, L"IdentificadorVentana", L"Controles", WS_OVERLAPPEDWINDOW | WS_SYSMENU,
        400, 80, 580, 630, HWND_DESKTOP, NULL, hIns, NULL);

    CreateWindowW(L"Static", L"Ingrese un numero : ", WS_VISIBLE | WS_CHILD | SS_NOTIFY, 90, 100, 150, 20, ventana, (HMENU)ID_LABEL, hIns, NULL);
    CreateWindowEx(0, "EDIT", "", ES_NUMBER | ES_AUTOHSCROLL | ES_LEFT | WS_CHILD | WS_VISIBLE, 250, 100, 40, 20, ventana, NULL, hIns, NULL);
    CreateWindowW(L"Static", L"Ingrese un numero : ", WS_VISIBLE | WS_CHILD, 90, 150, 150, 20, ventana, NULL, hIns, NULL);
    CreateWindowEx(0, "EDIT", "", ES_NUMBER | ES_AUTOHSCROLL | ES_LEFT | WS_CHILD | WS_VISIBLE, 250, 150, 40, 20, ventana, NULL, hIns, NULL);

    ShowWindow(ventana, nCmdShow);
    UpdateWindow(ventana);

    while (GetMessage(&mensaje, NULL, 0, 0) > 0) {
        TranslateMessage(&mensaje);
        DispatchMessage(&mensaje);
    }
}

LRESULT CALLBACK ProcedimientoVentana(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam) {
    static HINSTANCE Instancia;
    switch (msg) {
    case WM_CREATE: {
        Instancia = ((LPCREATESTRUCT)lParam)->hInstance;
        break;
    }

    case WM_COMMAND: {
        switch (LOWORD(wParam)) {
        case ID_LABEL: {
            printf("Has hecho un click\n"); // Cambiamos "echo" por "hecho"
            break;
        }
        }

        break;
    }

    case WM_DESTROY: {
        PostQuitMessage(0);
        break;
    }

    case WM_CTLCOLORSTATIC: {
        HDC hdcStatic = (HDC)wParam;
        SetTextColor(hdcStatic, RGB(0, 0, 0));
        SetBkColor(hdcStatic, RGB(21, 232, 189));
        return (LRESULT)CreateSolidBrush(RGB(21, 232, 189)); // Cambiamos INT_PTR por LRESULT
        break;
    }

    default: {
        return DefWindowProc(hwnd, msg, wParam, lParam);
    }
    }
    return 0;
}

