# Participant Guide - Building Agentic AI Applications in Snowflake

## Welcome to the Multi-User Cortex AI Lab! 🚀

This comprehensive guide will walk you through building an intelligent AI agent that can answer questions about both structured data (sales analytics) and unstructured data (product documents and images).

## Before You Start

### Prerequisites Checklist
- [ ] Received lab credentials (username, password, account URL)
- [ ] Downloaded the lab repository to your local machine
- [ ] Can access Snowflake with your assigned `CORTEX_LAB_USER` role
- [ ] Have a text editor or IDE for viewing/editing files

### What You'll Build
By the end of this lab, you'll have:
1. **Document Processing Pipeline** - Extract insights from PDFs and images
2. **Sales Analytics Dataset** - Structured data for business intelligence
3. **AI-Powered Search** - Semantic search across all your data
4. **Conversational Interface** - Streamlit app for natural language queries

---

## Part 1: Environment Setup (15 minutes)

### Step 1.1: Login and Verify Access

1. **Login to Snowflake**
   - Use the account URL provided by your instructor
   - Username: Your assigned email address
   - Password: Temporary password (you'll be prompted to change it)

2. **Verify Your Role**
   ```sql
   SELECT CURRENT_ROLE(), CURRENT_DATABASE(), CURRENT_WAREHOUSE();
   ```
   - You should see `CORTEX_LAB_USER` as your role
   - Database should be `CORTEX_LAB_SHARED`
   - Warehouse should be `CORTEX_LAB_WH`

### Step 1.2: Create Your Workspace

1. **Choose Your Schema Name**
   - Pick a unique identifier (e.g., `USER01`, `JOHN_DOE`, `TEAM_A`)
   - Remember this name - you'll use it throughout the lab

2. **Run the Setup Script**
   - Open `setup.sql` in your downloaded repository
   - **IMPORTANT**: Replace `USER01` with your chosen schema name
   - Execute the entire script in Snowflake

3. **Verify Setup**
   ```sql
   SELECT CURRENT_SCHEMA();
   SHOW STAGES;
   ```
   - You should see your schema name
   - Two stages should exist: `docs` and `semantic_files`

### Step 1.3: Upload Files

**Method 1: Using Snowsight (Recommended)**

1. **Upload Document Files**
   - Navigate to: Data → Databases → CORTEX_LAB_SHARED → [Your Schema] → Stages
   - Click on the `docs` stage
   - Click "Upload Files"
   - Select ALL files from the `docs/` folder in your downloaded repository
   - Wait for upload to complete

2. **Upload Semantic Files**
   - Click on the `semantic_files` stage
   - Upload `semantic.yaml` and `semantic_search.yaml` from the repository root

**Method 2: Using SnowSQL (Alternative)**

```bash
# Connect to your account
snowsql -a [account] -u [username]

# Upload document files
PUT file://path/to/docs/* @docs/;

# Upload semantic files
PUT file://semantic*.yaml @semantic_files/;
```

3. **Verify Uploads**
   ```sql
   SELECT * FROM DIRECTORY('@docs');
   SELECT * FROM DIRECTORY('@semantic_files');
   ```

---

## Part 2: Document Processing (30 minutes)

### Step 2.1: Open the Lab Notebook

1. **Launch Jupyter/Notebook Environment**
   - Open `SETUP_TOOLS.ipynb` in your preferred environment
   - If using Snowflake Notebooks, upload the file first

2. **Run Initial Setup Cells**
   - Execute the first few cells to import libraries
   - Verify your current context is displayed correctly

### Step 2.2: Process PDF Documents

**What you're doing**: Extracting text from product specification PDFs using AI

1. **Preview Your Documents**
   ```sql
   SELECT * FROM DIRECTORY('@docs') WHERE RELATIVE_PATH LIKE '%.pdf';
   ```

2. **Extract Text from PDFs**
   - Run the `AI_PARSE_DOCUMENT` cell
   - This uses Snowflake's AI to extract structured text from PDFs
   - **Expected result**: Text content from all PDF files

3. **Create Text Chunks**
   - Run the text chunking cell
   - This breaks documents into searchable pieces
   - **Expected result**: `DOCS_CHUNKS_TABLE` with thousands of text chunks

### Step 2.3: Process Images

**What you're doing**: Generating descriptions of product images using multimodal AI

1. **Generate Image Descriptions**
   - Run the image processing cell
   - This uses Claude to describe each product image
   - **Expected result**: Descriptive text added to your chunks table

2. **Classify Content**
   - Run the classification cell
   - AI automatically categorizes content as "Bike" or "Snow" products
   - **Expected result**: All chunks have category labels

### Step 2.4: Create Search Service

**What you're doing**: Setting up semantic search across all your content

1. **Create Cortex Search Service**
   ```sql
   CREATE OR REPLACE CORTEX SEARCH SERVICE DOCS
   ON chunk
   ATTRIBUTES relative_path, category
   WAREHOUSE = CORTEX_LAB_WH
   TARGET_LAG = '1 hour'
   EMBEDDING_MODEL = 'snowflake-arctic-embed-l-v2.0'
   AS (
       SELECT chunk, chunk_index, relative_path, category
       FROM docs_chunks_table
   );
   ```

2. **Test Your Search**
   ```sql
   SELECT SNOWFLAKE.CORTEX.SEARCH_PREVIEW(
       'DOCS', 
       'mountain bike specifications'
   );
   ```

---

## Part 3: Sales Analytics Setup (20 minutes)

### Step 3.1: Create Business Data

**What you're doing**: Building a realistic sales dataset for analytics

1. **Create Product Catalog**
   - Run the `DIM_ARTICLE` creation cell
   - This creates a table of bikes and ski products with prices

2. **Generate Customer Data**
   - Run the `DIM_CUSTOMER` creation cell
   - This creates 5,000 synthetic customers with demographics

3. **Create Sales Transactions**
   - Run the `FACT_SALES` creation cell
   - This generates 10,000 sales transactions over 3 years

### Step 3.2: Setup Analytics Search

**What you're doing**: Enabling natural language queries on your sales data

1. **Create Article Name Search**
   ```sql
   CREATE OR REPLACE CORTEX SEARCH SERVICE ARTICLE_NAME_SEARCH
     ON ARTICLE_NAME
     WAREHOUSE = CORTEX_LAB_WH
     TARGET_LAG = '1 hour'
     EMBEDDING_MODEL = 'snowflake-arctic-embed-l-v2.0'
   AS (
     SELECT DISTINCT ARTICLE_NAME
     FROM DIM_ARTICLE
   );
   ```

2. **Verify Your Data**
   ```sql
   SELECT 
       COUNT(*) as total_sales,
       SUM(total_price) as total_revenue,
       COUNT(DISTINCT customer_id) as unique_customers
   FROM FACT_SALES;
   ```

---

## Part 4: Deploy Your AI Agent (15 minutes)

### Step 4.1: Launch Streamlit App

1. **Open Terminal/Command Prompt**
   - Navigate to your `multi-user-lab/` folder

2. **Install Dependencies** (if needed)
   ```bash
   pip install streamlit snowflake-snowpark-python streamlit-extras
   ```

3. **Run the Application**
   ```bash
   streamlit run streamlit_app.py
   ```

4. **Access Your App**
   - Open the URL shown in terminal (usually `http://localhost:8501`)
   - Verify your current context is displayed in the sidebar

### Step 4.2: Test Your AI Agent

**Try these example queries:**

**Unstructured Data Queries** (searches documents/images):
- "What are the specifications of the Ultimate Downhill Bike?"
- "Show me information about ski boots"
- "What colors are available for the Premium Bicycle?"

**Structured Data Queries** (generates SQL):
- "What are our total sales by product category?"
- "Which customers bought the most expensive items?"
- "Show me monthly revenue trends for the last year"

**Mixed Queries** (uses both data types):
- "What are the best-selling bike models and their specifications?"
- "Compare sales performance of different ski brands"

---

## Part 5: Explore and Experiment (20 minutes)

### Advanced Features to Try

1. **Custom Prompts**
   - Modify the response instructions in `streamlit_app.py`
   - Try different conversation styles or focus areas

2. **Additional Data**
   - Add your own documents to the `@docs` stage
   - Create additional product categories or customer segments

3. **Enhanced Analytics**
   - Create new calculated fields in your semantic model
   - Add time-based filters or geographic analysis

### Troubleshooting Common Issues

**"Schema not found" errors**
- Verify you're in the correct schema: `USE SCHEMA [your_schema_name];`

**"Stage not found" errors**
- Check file uploads: `SELECT * FROM DIRECTORY('@docs');`
- Re-upload files if necessary

**"Warehouse not available" errors**
- Verify warehouse access: `USE WAREHOUSE CORTEX_LAB_WH;`

**Cortex functions not working**
- Check your role has proper permissions
- Contact instructor if persistent issues

**Streamlit connection issues**
- Ensure you're logged into Snowflake in your browser
- Check that your session hasn't expired

---

## Understanding What You Built

### Architecture Overview

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Documents     │    │   Cortex Search  │    │   Streamlit     │
│   (PDFs/Images) │───▶│   Service        │───▶│   Interface     │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                        │
┌─────────────────┐    ┌──────────────────┐           │
│   Sales Data    │    │   Cortex         │           │
│   (Tables)      │───▶│   Analyst        │───────────┘
└─────────────────┘    └──────────────────┘
```

### Key Technologies Used

- **Snowflake Cortex AI**: LLM functions for text processing and analysis
- **Cortex Search**: Semantic search across unstructured data
- **Cortex Analyst**: Natural language to SQL conversion
- **Streamlit**: Web application framework for the user interface
- **Snowpark**: Python integration with Snowflake

### Business Value

This architecture enables:
- **Unified Data Access**: Query both structured and unstructured data in natural language
- **Rapid Insights**: Get answers without writing SQL or searching through documents
- **Scalable AI**: Leverage enterprise-grade AI within your data platform
- **Cost Efficiency**: Process and analyze data without moving it outside Snowflake

---

## Next Steps

### Immediate Actions
1. **Save Your Work**: Export any custom queries or modifications
2. **Document Learnings**: Note what worked well and what could be improved
3. **Share Results**: Show colleagues what you built

### Continued Learning
1. **Explore Cortex AI Functions**: Try other AI capabilities in Snowflake
2. **Production Deployment**: Learn about scaling this architecture
3. **Advanced Features**: Investigate agents, fine-tuning, and custom models

### Resources
- [Snowflake Cortex AI Documentation](https://docs.snowflake.com/en/user-guide/snowflake-cortex)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [Snowpark Python Guide](https://docs.snowflake.com/en/developer-guide/snowpark/python/index)

---

## Feedback

Your feedback helps improve this lab! Please share:
- What worked well?
- What was confusing?
- What would you like to see added?
- How would you use this in your work?

**Contact**: [Instructor contact information]

---

**Congratulations!** 🎉 You've successfully built an intelligent AI agent that can reason across multiple data types. This foundation can be extended and customized for countless business applications.

Happy building! 🚀
