# Building Agentic AI Applications In Snowflake - Multi-User Lab

## Overview

This is the **multi-user lab version** designed for hands-on training environments where multiple participants share a sandbox Snowflake account. Each participant works in their own isolated schema while sharing common resources like warehouses and integrations.

## Lab Architecture

- **Shared Database**: `SNOWCAMP_DB` - accessible by all participants
- **Individual Schemas**: Each participant creates their own schema (e.g., `USER01`, `USER02`, etc.)
- **Shared Warehouse**: `SNOWCAMP_WHS` - used by all participants
- **Shared Integrations**: Email integration shared across all users
- **Individual Workspaces**: Each user has their own stages, tables, and Cortex services

## Prerequisites

### For Instructors
- ORGADMIN or ACCOUNTADMIN access to set up the lab environment
- Completed pre-lab setup (see `INSTRUCTOR_SETUP.md`)

### For Participants
- Access to the shared Snowflake account
- Assigned role: `SNOWCAMP`
- Downloaded lab repository to local machine

## Quick Start Guide

### Step 1: Initial Setup
1. **Download Repository**: Download this repository to your local machine
2. **Login to Snowflake**: Use your assigned credentials
3. **Verify Role**: Ensure you're using the `SNOWCAMP` role

### Step 2: Create Your Workspace
1. **Run Setup Script**: Execute `setup.sql` (modify the schema name to be unique)
2. **Upload Files**: Upload files from the `docs/` folder to your `@docs` stage
3. **Upload Semantic Files**: Upload `semantic.yaml` and `semantic_search.yaml` to your `@semantic_files` stage

### Step 3: Build Your AI Agent
1. **Open Notebook**: Work through `SETUP_TOOLS.ipynb`
2. **Process Documents**: Extract information from PDFs and images
3. **Create Data Tables**: Build the sales analytics dataset
4. **Setup Cortex Services**: Configure search and analytics tools

### Step 4: Configure Agent 
1. **Configure your Agent**: Create your agent using Cortex Analyst and Cortex Search Service
2. **Test Your Agent**: Ask questions about bikes, skis, and sales data
3. **Explore Features**: Try both structured and unstructured data queries

### Enhanced Multi-User Support
- **Individual Workspaces**: Each user has their own schema and objects
- **Shared Resources**: Common warehouse and integrations for cost efficiency
- **Dynamic Configuration**: Semantic models adapt to current user context
- **Context Awareness**: Streamlit app shows current user's database/schema/role

## File Structure

```
multi-user-lab/
├── README.md                    # This file - main lab overview
├── INSTRUCTOR_SETUP.md          # Pre-lab setup for instructors
├── PARTICIPANT_GUIDE.md         # Detailed step-by-step instructions
├── FILE_UPLOAD_GUIDE.md         # File upload instructions
├── setup.sql                    # User workspace setup script
├── SETUP_TOOLS.ipynb            # Main lab notebook
├── streamlit_app.py             # AI agent application
├── semantic.yaml                # Data model definition
├── semantic_search.yaml         # Enhanced semantic model with search
├── environment.yml              # Python dependencies
├── LICENSE                      # License file
└── docs/                        # Sample documents and images
    ├── *.pdf                    # Product specification documents
    └── *.jpeg                   # Product images
```

## Learning Objectives

By the end of this lab, you will have:

1. **Built a Multi-Modal AI Agent** that can reason over both structured and unstructured data
2. **Implemented Cortex Search** for document and image processing
3. **Created Cortex Analyst** for natural language SQL generation
4. **Deployed a Streamlit Application** with conversational AI interface
5. **Understood Multi-Tenant Architecture** in Snowflake environments

## Troubleshooting

### Common Issues

**Schema Not Found**
- Ensure you've run `setup.sql` and created your schema
- Verify you're using the correct schema name throughout the lab

**Files Not Found**
- Check that files are uploaded to the correct stages (`@docs`, `@semantic_files`)
- Use `SELECT * FROM DIRECTORY('@docs');` to verify uploads

**Permission Errors**
- Confirm you're using the `CORTEX_LAB_USER` role
- Contact instructor if you need additional permissions

**Cortex Services Not Working**
- Ensure warehouse `CORTEX_LAB_WH` is available and running
- Check that semantic files are properly uploaded and formatted

### Getting Help

1. **Check Participant Guide**: Detailed instructions in `PARTICIPANT_GUIDE.md`
2. **File Upload Issues**: See `FILE_UPLOAD_GUIDE.md`
3. **Ask Instructor**: For permissions or infrastructure issues
4. **Check Logs**: Use Snowflake query history for debugging SQL issues

## Next Steps

After completing this lab:

1. **Explore Advanced Features**: Try different Cortex AI functions
2. **Customize Your Agent**: Modify prompts and add new data sources
3. **Scale Your Solution**: Learn about production deployment patterns
4. **Join Community**: Connect with other Snowflake AI developers

## Resources

- [Snowflake Cortex AI Documentation](https://docs.snowflake.com/en/user-guide/snowflake-cortex)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [Original Lab Repository](https://github.com/Snowflake-Labs/sfguide-build-data-agents-using-snowflake-cortex-ai)

---

**Happy Learning!** 🚀

For questions or issues, please reach out to your lab instructor.
