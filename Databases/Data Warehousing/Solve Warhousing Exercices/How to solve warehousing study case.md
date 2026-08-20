The case studies reliably follow a 5-step structure. If you memorize this framework, you can apply it to any scenario.

# Rule 1: Extract Clues from the "Objectives" Paragraph

Before looking at the database schema, read the bulleted list of objectives in the prompt carefully. This list gives you the answers to the first two questions:

- **The "By" or "Per" words (en fonction de, par):** These are your **Dimensions** (e.g., "par grade", "par trimestre", "par programme").
- **The "What" words (le suivi de, le nombre, le taux):** These are your **Measures/Facts** (e.g., "taux d'absentéisme", "nombre d'inscriptions").

# Rule 2: How to Propose Dimensions

When listing your dimensions, use the prefix `Dim_` and group related attributes together.

- **Advice:** **Always include a Time dimension (****Dim_Temps****).** Even if there isn't a dedicated "Time" table in the operational database, you will always need to analyze data over time (e.g., using `date_inscription` or `Dcng_datedeb`).
- **Advice:** Dénormalize the operational tables. For example, if the source has separate tables for `Département` and `Programme`, combine them into a single `Dim_Programme` for your star schema.

# Rule 3: How to Describe the Fact Table

The fact table (`Fact_NomDuProcessus`) sits at the core of your warehouse. You must always list two specific things to get full marks:
1. **Foreign Keys (Clés étrangères):** List an ID for _every single dimension_ you proposed in step 1 (e.g., `id_temps`, `id_employé`, `id_cours`).
2. **Measures (Mesures):** List the numeric values you will calculate.
    - **Advice:** Ensure your measures are quantifiable (calculable). Use counts (nombre d'inscriptions), sums (total payé), or differences (date_fin - date_début pour les jours d'absence).

# Rule 4: How to Propose Hierarchies (Question 3)

Hierarchies define the "Roll-up" and "Drill-down" paths. Always structure them from the most detailed level to the most general level.

- **Standard Time Hierarchy:** Date → Mois (Month) → Trimestre (Quarter) → Année (Year).
- **Standard Geography Hierarchy:** Adresse → Ville → Province/Pays.
- **Standard Organizational Hierarchy:** Employee → Poste → Département.

# Rule 5: How to Draw the Star Schema (Question 4)

The exam explicitly asks for a **Star Schema** (Schéma en étoile).

- **Rule:** Draw one central box for your Fact table containing the foreign keys and measures.
- **Rule:** Draw your Dimension tables surrounding the fact table.
- **Crucial Advice:** Connect the primary keys of your dimensions _directly_ to the central fact table. Do not connect dimension tables to each other (that would make it a snowflake schema, which is incorrect for this prompt).

# Rule 6: How to Schematize the ETL Process (Question 5)

You must specify the sources and destinations for the data flows. Draw a diagram with three distinct zones:

1. **Extraction (Source):** On the left, list the exact names of the operational tables provided in the exam's ER diagram (e.g., `Grh_Employes`, `InscriptionCours`).
2. **Transformation (Transit Zone):** In the middle, you must mention cleaning the data, but more importantly, **state the mathematical rules for your measures**. For example, write "Calculate days absent = End Date - Start Date" or "Calculate Group Size = Count of students".
3. **Loading (Destination):** On the right, point the arrows to your newly designed Star Schema (Fact and Dimension tables).

If you follow this exact blueprint, you will address every requirement the grader is looking for in the Case Study section.

Would you like to practice this methodology on a hypothetical case study?