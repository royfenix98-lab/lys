# ly-generator

Genera los Layouts de Pago (LY) en Excel a partir de XML, PDF (incluso unidos), imagenes y estados de cuenta.

| Flujo | Comando |
|---|---|
| 1. Gastos fijos | `python -m lygen gastos-fijos --xml luz.xml agua.xml --pdf luz.pdf agua.pdf` |
| 2. Proveedores | `python -m lygen proveedores --xml *.xml --pdf *.pdf --oc oc.csv --obs obs.csv` |
| 3. Devoluciones | `python -m lygen devoluciones --paquete paquete_unido.pdf` |
| 4A/4B. Viaticos | `python -m lygen viaticos --entrada solicitudes.csv` |

Opciones comunes: `--catalogo libro.xlsm` (hoja *Proveedores* y *CONFIG*), `--config config.json`, `--altas altas_catalogo.xlsx`, `--salida LY.xlsx`, `--modo pago|trabajo`, `--interactivo` (pregunta antes de dar de alta en el catalogo).

## Instalacion
```bash
python -m venv .venv && source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```
OCR (INE y escaneos): instalar **Tesseract** con el idioma espanol.
* Windows: instalador de UB-Mannheim (marcar *Spanish*) y agregarlo al PATH.
* Ubuntu: `sudo apt install tesseract-ocr tesseract-ocr-spa`.

## Uso rapido
```bash
cp config/config.example.json config/config.json        # ajusta reglas de agencia, umbrales...
python -m lygen devoluciones --paquete scans/paquete.pdf --catalogo data/LY_PAGOS_DIARIOS.xlsm \
       --config config/config.json --salida salidas/LY_devoluciones.xlsx --interactivo
```
El Excel trae la hoja **LY** y la hoja **INCIDENCIAS** (BLOQUEO = el registro no se escribio; AVISO = revisalo). Codigo de salida `1` si hubo bloqueos.

**Viaticos**: ver `config/viaticos_ejemplo.csv` (columnas `tipo, beneficiario, monto, estado_cuenta, razon_social, agencia, autorizado_por, detalle, provisiones, fecha`; en reembolso las provisiones van separadas por coma, `;` o salto de linea).

## Pruebas
```bash
pip install -r requirements-dev.txt
python -m unittest discover -s tests -t . -v      # o: pytest
```

## Documentacion
`docs/ARQUITECTURA.md`: estructura, reglas por flujo, regla global de catalogo, librerias, migracion del VBA y supuestos.

> **Privacidad**: INE, estados de cuenta y el catalogo contienen datos personales. No los subas al repositorio (`.gitignore` ya excluye `*.xlsm`, `data/` y las altas).
