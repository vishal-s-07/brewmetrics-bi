# Reflection

GitHub Copilot was useful throughout the BrewMetrics BI project, especially for generating DAX measures and drafting project documentation. Copilot helped provide initial suggestions for MoM Sales Growth %, Running Total Sales, Item Sales Rank, and Average Transaction Value. These suggestions reduced the time required to write the measures and helped me understand the structure of the DAX expressions.

However, the Copilot output still required verification and correction. I checked the generated DAX against the actual Power BI semantic model and verified the measures in the report. While preparing the README, Copilot initially described a different dashboard structure and stated that the report contained one page. I reviewed the actual Power BI report and corrected the README so that the documented visuals, slicer, drill-down hierarchy, and insights matched the final dashboard.

Maintaining the complete Git commit history also made the development process easier to track. Each major stage was saved separately, including the star schema, individual DAX measures, and the dashboard. This provides a clear record of how the BI solution developed from the initial model into the final report. It also makes it easier to identify which changes were made at each stage and supports an auditable development workflow.
