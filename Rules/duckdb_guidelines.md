---
description: Guidelines for using DuckDB with Python, emphasizing a DuckDB-first approach and strict policies for Pandas usage.
author: JD
version: 1.0
tags: ["duckdb", "python", "guideline", "database", "best-practices"]
---
# LLM Guidelines for DuckDB and Python Projects

## DuckDB is the Primary Tool

The primary engine for all data manipulation, querying, and analysis in this project is **DuckDB**. Prioritize using DuckDB's capabilities for all data-related tasks. The goal is to leverage its performance and SQL-based approach.

## Idiomatic DuckDB-Python Usage

*   **Connection:** Always use `con` as the variable for the DuckDB connection (`con = duckdb.connect(...)`).
*   **Data Ingestion:** Do not load data into a pandas DataFrame first. Load data directly into DuckDB tables using its native functions.
    *   **Correct:** `con.sql("CREATE TABLE my_table AS SELECT * FROM read_csv_auto('my_file.csv')")`
    *   **Incorrect:** `df = pd.read_csv('my_file.csv'); con.register('my_table', df)`
*   **Data Transformation:** Perform all transformations (filtering, aggregation, joins, cleaning) using DuckDB SQL queries via `con.sql()` or `con.execute()`. Avoid pulling data into pandas to perform these steps.

## Strict Pandas Usage Policy

*   **When to Use Pandas:** Pandas should only be used as a "last mile" tool, primarily for:
    *   Data visualization libraries that require a pandas DataFrame as input.
    *   Complex, stateful operations that are significantly more difficult or impossible to express in SQL.
*   **Ask Before Using:** Before converting a DuckDB table or query result into a pandas DataFrame (e.g., using `.df()`), you **must** ask the user for permission. Your request should include:
    *   Which data you intend to convert
    *   A clear justification for why a pandas DataFrame is necessary for the next step

## Best Practices and Other Ideas

*   **Leverage DuckDB Functions:** Actively use DuckDB's rich set of SQL functions and extensions (e.g., for JSON, dates, window functions, `PIVOT`/`UNPIVOT`). Do not assume standard SQL is the limit.
*   **Use CTEs:** For complex queries, use Common Table Expressions (CTEs) (`WITH ... AS (...)`) to improve readability and modularity.
*   **Limit Exploratory Queries:** When exploring data, always use `LIMIT` in your queries to avoid unintentionally printing thousands of rows and cluttering the notebook.
*   **In-Memory First:** Default to using an in-memory database (`:memory:`) for speed, unless the user explicitly requests data persistence across sessions.

## Choosing the Right Tool: Tables vs. Views vs. CTEs

To keep the analysis clean, performant, and readable, use the following conventions for storing and structuring your data and queries.

*   **Use `TABLE`s for Foundational Data:**
    *   **Raw Data:** The initial, raw data loaded from external sources (like `read_csv_auto`) should always be a table. This is your source of truth.
    *   **Materialized Steps:** Create intermediate tables for significant, computationally expensive transformation steps that you plan to query multiple times. This avoids re-computing the same logic repeatedly.

*   **Use `VIEW`s for Reusable Logic:**
    *   **Cleaned Data:** Create views to represent cleaned-up or standardized versions of your base tables (e.g., `CREATE VIEW cleaned_expenses AS ...`).
    *   **Business Logic:** Use views to encapsulate specific, reusable business logic or aggregations (e.g., a view that calculates `monthly_category_totals`).
    *   **Simplicity:** Views are stored queries, not stored data. They are perfect for simplifying access to complex underlying queries without using extra memory.

*   **Use CTEs (`WITH` clauses) for In-Query Steps:**
    *   **Readability:** Use Common Table Expressions (CTEs) to break down a single, complex query into logical, readable steps.
    *   **Single Use:** CTEs are scoped only to the query they are part of. They are ideal for multi-step transformations or aggregations that you don't need to reuse in other queries.

## Notebook Structure and Flow

To ensure notebooks are readable, maintainable, and easily navigable, follow these guidelines:

*   **Markdown Cells for Sections:** Precede all major sections of your notebook with a Markdown cell.
*   **Headings:** Use H2 headings (e.g., `## Section Title`) for major sections. Use subsequent headings (H3, H4, etc.) for sub-sections as needed.
*   **Descriptive Text:** Include a small amount of descriptive text within these Markdown cells to explain the purpose and content of the upcoming code or analysis. This helps the notebook flow as a cohesive document.
*   **Table of Contents:** Structuring your notebook with proper headings will allow external applications (like VS Code's Markdown preview) to generate a table of contents, improving navigability.

## SQL Formatting Conventions

*   **F-string Triple-Quoted SQL:** For all SQL queries executed via `con.execute()` or `con.sql()`, use f-string triple-quoted strings (`f\"\"\"...\"\"\"`). This allows for easy multi-line queries and variable interpolation.
*   **Indentation and Alignment:**
    *   Indent the SQL query within the triple quotes by one level (typically 4 spaces or one tab) relative to the `con.execute(f"""` line.
    *   Align SQL keywords (e.g., `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `ORDER BY`) for readability.
    *   Align column names and other clauses within their respective sections.

    **Example:**
    ```python
    con.execute(f\"\"\"
                SELECT
                    column1,
                    column2
                FROM
                    my_table
                WHERE
                    column1 = '{some_variable}'
                GROUP BY
                    column1
                ORDER BY
                    column2;
                \"\"\")
