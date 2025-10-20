---
applyTo: "**"
---

Provide project context and coding guidelines that AI should follow when generating code, answering questions, or reviewing changes.
The aim of the code is to just move data quickly from outside a project in CSVs into the database using Python and data science.
Mostly using pandas to read CSVs and requests to make API calls to the database.
Be sure to use best practices for code quality, readability, and maintainability.
Use the following libraries where appropriate:

- pandas
- requests
- json
- os
- dotenv
- time
- logging
- typing

Ensure the code handles errors gracefully, including network issues, data inconsistencies, and API errors.
Implement logging to track the progress of data migration and any issues encountered.
Use environment variables to manage sensitive information like API keys and database credentials.

Comment the code thoroughly to explain the logic and flow.
Structure the code into functions or classes to enhance modularity and reusability.
At the end of the task, provide a summary of what was done and any recommendations for future improvements or considerations.
When generating code, answering questions, or reviewing changes, always refer to this document for instructions.
Project Context and Coding Guidelines
The project involves migrating data from CSV files into a database using Python, with a focus on data science techniques. The primary libraries to be used include pandas for data manipulation and requests for making API calls to the database.

Key Requirements:

1. Data Migration: Efficiently read data from CSV files and insert it into the database.
2. Error Handling: Implement robust error handling to manage network issues, data inconsistencies, and API errors.
3. Logging: Set up logging to monitor the progress of the data migration process and capture any issues encountered.
4. Environment Variables: Use environment variables to securely manage sensitive information such as API keys and database credentials.
   Coding Guidelines:
5. Code Quality: Follow best practices for code quality, readability, and maintainability.
6. Modularity: Structure the code into functions or classes to enhance modularity and reusability.
7. Documentation: Comment the code thoroughly to explain the logic and flow of the program.
8. Libraries: Utilize the specified libraries (pandas, requests, json, os, dotenv, time, logging, typing) where appropriate.
9. Summary: At the end of the task, provide a summary of the work done and any recommendations for future improvements or considerations.
   By adhering to these guidelines and requirements, the project aims to achieve a smooth and efficient data migration process while maintaining high standards of code quality and security.

The following are the endpoints and authentication details for the API:

- Base URL: env variable API_BASE_URL
- Authentication: Bearer token in env variable API_TOKEN

Do not execute the notebook cells. Only provide code snippets as needed.

For every conversation, create a new file named conversation\_<unique_id>.md in the .github/conversations/ directory to log a summary of the interaction and any important details discussed.
Refer to the conversation log for context in future interactions.
