**Roll-up** allows you to consolidate or summarize data by moving up to a higher level in a dimension's hierarchy.

- **Advice:** Use this operation when you want a broader, generalized view of your data.
- **Example:** In a hotel reservation cube analyzing participants by "Age", you can perform a roll-up to summarize the individual ages into broader "Age groups" (tranche d'âge). Similarly, in a commercial cube, you can roll up "Suppliers" to see total purchases by "Region" instead of by individual names.

**Drill-down** is the exact inverse of a roll-up; it allows you to navigate to more detailed data by descending a dimension's hierarchy.

- **Advice:** Use this when you spot a high-level trend and need to investigate the granular details, or when you need to introduce a new dimension into your current view.
- **Example:** If you are viewing total hotel nights by "Year", performing a drill-down on the time dimension allows you to see the specific breakdown by "Month". You can also use a drill-down to add a completely new dimension, such as adding "Activity" to your cube so you can isolate data for "Formation" events.

**Slice** is a projection and selection operation that filters the multidimensional model on exactly **one single dimension**.

- **Advice:** Use this to isolate a specific category or time period so you can analyze all other dimensions through that single filter.
- **Example:** Taking a commercial sales cube and applying a slice on the "Date" dimension to only look at total purchases for the year "2024".

**Dice** is an extraction operation that defines a smaller sub-cube by applying filters on **two or more dimensions** simultaneously.

- **Advice:** Use this when you need a highly specific intersection of data points to answer a targeted business question.
- **Example:** Filtering a commercial sales cube to extract data only for the specific months of "January" and "February" across specific products and specific suppliers.

**Pivot (Rotate)** reorients the visual representation of the cube.

- **Advice:** Use this operation strictly for visualization purposes when you want to swap your rows and columns or change how the data is displayed to the user, without changing the underlying calculations or applying new filters.