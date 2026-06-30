# Entregable · Sesión 3 — Copilotos IA

- **Nombre / usuario:*Diego Rodriguez*
- **Fecha de entrega:*29/Julio*
- **Repo auditado en la Parte A** (solo tipo/contexto, NO el código): _p. ej. "Projecto databricks con dos años de evolución"_

---

## 1. Hallazgos de la auditoría (Parte A)

> 3-5 cosas que el agente **no pudo inferir** del código y que tendrías que decirle explícitamente.
> Redáctalas para que otra persona las entienda sin contexto adicional. **Sin código propietario ni secretos.**

1.	El sistema utiliza el schema config, como input en las etapas de validación referencial y estructural.
2.  El sistema utiliza el schema control, para llevar registro de los logs de las ejecuciones de cada layer y resultados los datos ingestados.
2.	El naming convetion utilizado en el código Python es Pascal Case.
3.	El storage account es utilizado por el sistema para lectura por parte de la capa de bronze todos los contents a procesar. 
4.	El storage account existente es utilizado como salida del proceso de Posting & Hardening

## 2. SKILL.md de la skill creada (Parte B)

```markdown
name: generate-validation-test
description: Use this skill to generate a validation test for a given validation function or class. This will create databricks notebook.
It receives a parameter with the catalog. schema name . To execution the validation test, it includes, creation of temporary schema,
a set of create table used by the validation, all the required inserts of valid or invalid data to execution the positive a negative test cases.
Use this skill to generate a validation test for a given validation function or class. This will create databricks notebook.
It receives a parameter with the catalog. schema name.

# Identifying the validations.

1. When the skill starts, asks the user the list of validations files to test.
2. Read the devops repository/branch url from the project.
3. Requires de XLS file with the description of the every validation.
3. Requeries the list of task url <TASK_URL>:
  - https://dev.azure.com/enterprise1/CORI/_workitems/edit/
4. For each validation:
    - Run 'git diff -- staged" to read the staged changes.
    - Check the description for the current validation, reading the sheet named "Validations" and the get the full rows description base on column A.
    - Analyize the diff and generate a description message between the previous version and the new version.
    - Requires the bug id/ task id from DEVOPS, <idtask>.
    - Get from devops <TASK_URL>/<idtask>/ the description of the task.
      Compare the current code diff with the <idtask> and validation description in XLS file.
    - If any unconsysency scenario are finding, generate a list with the description findings and stop the process for the current validation.
    - The notebook name should be created with this pattern: Test_ + VALIDATION_ID + <idtask>, if the notebook exists, It will be overwritten.
    - If all the escription are consistent generates a validation notebook with the description of the changes.
    - The notebooke code includes:
          - The code to create the set of temporary tables used in every validation the validation.
          - The insert to load all necessary data for the required inserts of valid or invalid data to execute the positive and negative test cases.
          - A sumary of the test cases results.
          - Code to drop all the temporary schema and tables created for the validation test.
    
# Rules
- A1l validations will be stored in the "_Validations/00_Ingestion/020_Silver/Validations/" folder under the root folder.
- All noteboooks will have two paremeters: catalog. schema and drop temporal data. The first one is the catalog. schema name where the validation will be executed. The second one is a boolean parameter to drop all the temporal
```

## 3. Diario de decisiones

*Skill creada:* [generate-validation_test: genera las test sobre la validación cruzada de datos en base a reglas de negocio.]
(https://github.com/stdrodriguez/ai4devs-auditoria-contexto-creacion-skill-s3-202606-seniors-ii/tree/main]

*Decisiones de diseño tomadas:*
- Decisión 1: Analysis de la coherencia de las definiciones y codigo generado para la validación.
- Decisión 2: Generación de casos de test en base a datos temporales creados especificacmente para la validación.
- Decisión 3:

*Qué me resultó fácil:*
- Entender la idea general de la estructura parecio simple.
- 
*Qué me resultó ambiguo o difícil de decidir:*
- Cerrar mentalmente la idea, El la practica la documentación 100% muchas veces no es posible, porque el dueño de los documentos luego de aclaradas las dudas no siempre actualiza los mismo.
- Al momento de ponerme a pensar en todos las entradas posibles para evitar inconsistencias y alinear la documentación al codigo, no fue muy intuitivo ver las alternativas. En la practia,se resuleva la duda y se continua.

*Tiempo real invertido:*
- 3 horas, en hacer la skill, mas otras 3 horas de lectura y comparación documentos.

*Qué probarías si tuvieras más tiempo:*
-entraria en un loop de prueba y corrección para el caso planteado.}

*¿Usaste IA para crear la skill?* (qué partes generaste con IA y qué partes decidiste tú)
- Si la usaria, no se me ocurrió, generé todo a mano.

### Resultado de la prueba (Paso 8)

- ¿Se activó cuando lo esperabas?
- si correcto.
- ¿El resultado fue el que querías?
- Si aunque tuve que hacer luego una segunda versión para que si no existe cambiosen la branch, saque la definición del codigo y la verifique contra la documentación y tarea definida.
- Si no, ¿qué crees que falló? (no la "arregles" — documenta el primer intento)
- La primer versión no dió error, solo que al momento de ejecutarla unos de los requerimientos definidos era leer la rama con los cambios, pero no existian, por lo tanto cambie el comportamiento. 
