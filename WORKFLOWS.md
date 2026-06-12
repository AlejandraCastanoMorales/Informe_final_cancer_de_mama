# DIAGRAMA DE CAJAS PARA REPRESENTAR EL FLUJO DEL PROYECTO

A continuación se ilustra el flujo de trabajo completo, desde la descarga de datos hasta el entrenamiento del modelo y su evaluación analítica.

```mermaid
graph TD
    %% --- DEFINICIÓN DE ESTILOS SEGUROS PARA VS CODE ---
    classDef inicioFin fill:#263238,stroke:#000,stroke-width:2px,color:#fff;
    classDef instalacion fill:#f5f5f5,stroke:#9e9e9e,stroke-width:2px,stroke-dasharray:5 5,color:#616161;
    classDef baseDatos fill:#43A047,stroke:#1B5E20,stroke-width:2px,color:#fff;
    classDef procesoCritico fill:#D81B60,stroke:#880E4F,stroke-width:2px,color:#fff;
    classDef visualizacion fill:#FFB300,stroke:#FF6F00,stroke-width:2px,color:#000;
    classDef modelo fill:#3F51B5,stroke:#1A237E,stroke-width:2px,color:#fff;
    classDef evaluacion fill:#00BCD4,stroke:#006064,stroke-width:2px,color:#fff;

    %% --- BLOQUES DEL PIPELINE (FORMATO ESTÁNDAR UNIVERSAL) ---
    Start[" INICIO"]:::inicioFin
    
    Step1["1. Instalación de dependencias externas"]:::instalacion
    
    Step2["2. Carga de librerías científicas y Scikit-Learn"]:::instalacion
    
    DataBlock["3. REPOSITORIO UCI - Descarga remota del dataset"]:::baseDatos
    
    Step4["4. AUDITORÍA - Inspección de nulos y duplicados"]:::baseDatos
    
    Step5["5. INGENIERÍA DE FEATURES - Eliminación de ID y Binarización"]:::procesoCritico
    
    Step6["6. ESCALAMIENTO - Normalización con StandardScaler"]:::procesoCritico
    
    Step7["7. REDUCCIÓN PCA - Compresión de 30 variables a 2D"]:::procesoCritico
    
    Graph["8. VISUALIZACIÓN - Generación de Scatter Plot interactivo"]:::visualizacion
    
    Step9["9. SELECCIÓN DE MODELO - Ajuste del algoritmo óptimo a los datos"]:::modelo
    
    Step10["10. CURVAS DE ENTRENAMIENTO - Gráficos de AUC, F1 y Recall"]:::evaluacion
    
    Step11["11. MATRIZ DE CONFUSIÓN - Evaluación final de clasificación"]:::evaluacion
    
    End["🏁 MODELO ENTRENADO Y EVALUADO"]:::inicioFin

    %% --- FLUJO DE FLECHAS ---
    Start --> Step1
    Step1 --> Step2
    Step2 --> DataBlock
    DataBlock --> Step4
    Step4 --> Step5
    Step5 --> Step6
    Step6 --> Step7
    Step7 --> Graph
    Graph --> Step9
    Step9 --> Step10
    Step10 --> Step11
    Step11 --> End
