# LEGO Data Analysis

## Overview
This project explores a dataset about LEGO, answering various questions about the history, product offerings, and trends in LEGO sets over time. Using Python and Pandas, we analyze key insights, including:

- The **largest LEGO set** ever created and its number of parts.
- The **earliest LEGO sets**, their release year, and the number of sets sold.
- The **most popular LEGO theme** based on the number of sets.
- The **growth of LEGO over time**, tracking annual set releases and themes.
- The **evolution of LEGO set complexity**, examining set sizes and part counts over time.

## Dataset
The analysis uses the following CSV files:
- `colors.csv`: Contains LEGO brick colors.
- `sets.csv`: Lists LEGO sets with release year and number of parts.
- `themes.csv`: Details LEGO themes and their associated sets.

## Key Analyses

### 1. Exploring LEGO Brick Colors
- **Total unique colors** in LEGO sets.
- **Count of transparent vs. opaque colors.**

```python
colors = pd.read_csv('Day74/colors.csv')
print(colors['name'].nunique())
print(colors.groupby('is_trans').count())
```

### 2. Exploring LEGO Sets
- First LEGO set release year and product names.
- Number of LEGO sets sold in the first year.
- Top 5 LEGO sets with the most parts.

```python
sets = pd.read_csv('Day74/sets.csv')
print(sets.sort_values('year', ascending=True).head())
print(sets.loc[sets['year'] == 1949])
print(sets.sort_values('num_parts', ascending=False).head())
```

### 3. Growth of LEGO Over Time
- Number of sets released annually.
- Plot showing LEGO’s growth trajectory.

```python
sets_by_year = sets.groupby('year').count()
plt.figure(figsize=(10,6))
plt.plot(sets_by_year.index, sets_by_year.set_num, marker='o', linestyle='-', color='b')
plt.xlabel('Year')
plt.ylabel('Count of Sets')
plt.title('Number of Sets per Year')
plt.grid(True, linestyle='--', alpha=0.6)
plt.show()
```

### 4. Number of Themes Over Time
- Unique LEGO themes per year.
- Parallel plot of themes and sets per year.

```python
themes_by_year = sets.groupby('year').agg({'theme_id': pd.Series.nunique})
themes_by_year.rename(columns={'theme_id': 'nr_themes'}, inplace=True)
plt.plot(themes_by_year.index[:-2], themes_by_year.nr_themes[:-2])
```

### 5. Complexity of LEGO Sets Over Time
- Calculation of average number of parts per set.
- Trend analysis of increasing LEGO set complexity.

```python
parts_per_set = sets.groupby('year')['num_parts'].mean()
plt.scatter(parts_per_set.index[:-2], parts_per_set.num_parts[:-2])
```

### 6. Most Popular LEGO Themes
- Identifying the LEGO theme with the most sets.
- Using `merge()` to combine datasets and analyze LEGO’s most successful themes.

```python
set_theme_count = sets["theme_id"].value_counts().reset_index()
set_theme_count.columns = ['id', 'set_count']
themes = pd.read_csv('Day74/themes.csv')
merged_df = pd.merge(set_theme_count, themes, on='id')
plt.figure(figsize=(14,8))
plt.bar(merged_df.name[:10], merged_df.set_count[:10])
plt.xticks(fontsize=14, rotation=45)
plt.ylabel('Number of Sets')
plt.xlabel('Theme Name')
plt.show()
```

## Conclusion
Lego’s product lineup includes **iconic themes** like **Star Wars, Town, and Ninjago**, but also an extensive range of **books, keychains, and other merchandise**. The **'Gear' category** is massive, raising questions about whether Lego is **straying from its core business** or **successfully diversifying**. This analysis provides insights into LEGO’s expansion, but a deeper business analysis could explore its long-term strategy further.