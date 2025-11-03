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

### 🏫 For Instructors
1. **Go to** `instructor-setup/` folder
2. **Read** `README.md` for quick overview
3. **Follow** `INSTRUCTOR_SETUP.md` for complete setup
4. **Create** user accounts and configure resources

### 👥 For Participants  
1. **Start in** `participant-setup/` folder
2. **Read** `README.md` for quick overview
3. **Follow** `PARTICIPANT_GUIDE.md` step-by-step
4. **Upload files** using `FILE_UPLOAD_GUIDE.md`
5. **Execute lab** in `lab-materials/` folder

### 🚀 Lab Flow (Participants)
1. **Setup** (15 min): Run `participant-setup/setup.sql` and upload files
2. **Lab Execution** (90 min): Work through `lab-materials/SETUP_TOOLS.ipynb`
3. **Deployment** (15 min): Launch in Snowflake Intelligence
4. **Testing** (10 min): Ask questions and explore your AI agent

### Enhanced Multi-User Support
- **Individual Workspaces**: Each user has their own schema and objects
- **Shared Resources**: Common warehouse and integrations for cost efficiency
- **Dynamic Configuration**: Semantic models adapt to current user context

## File Structure

```
multi-user-lab/
├── README.md                    # This file - main lab overview
├── LICENSE                      # License file
├── instructor-setup/            # 🏫 For instructors only
│   ├── README.md               # Instructor quick start guide
│   └── INSTRUCTOR_SETUP.md     # Complete pre-lab setup instructions
├── participant-setup/          # 👥 Start here as a participant
│   ├── README.md               # Participant quick start guide
│   ├── PARTICIPANT_GUIDE.md    # Step-by-step lab instructions
│   ├── FILE_UPLOAD_GUIDE.md    # Detailed file upload help
│   └── setup.sql               # Your workspace setup script
├── lab-materials/              # 🧪 Lab execution files
│   ├── README.md               # Lab materials overview
│   └── SETUP_TOOLS.ipynb       # Main lab notebook
├── config/                     # ⚙️ Configuration files
│   ├── README.md               # Configuration guide
│   ├── semantic.yaml           # Data model definition
│   ├── semantic_search.yaml    # Enhanced semantic model
│   └── environment.yml         # Python dependencies
└── sample-data/                # 📊 Sample documents and images
    ├── README.md               # Sample data overview
    └── docs/                   # Product catalogs (22 files)
        ├── *.pdf               # Product specifications
        └── *.jpeg              # Product images
```

## Learning Objectives

By the end of this lab, you will have:

1. **Built a Multi-Modal AI Agent** that can reason over both structured and unstructured data
2. **Implemented Cortex Search** for document and image processing
3. **Created Cortex Analyst** for natural language SQL generation
4. **Deployed a Cortex Agents** with conversational AI interface in Snowflake Intelligence
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
- [Snowflake Intelligence Documentation](https://docs.snowflake.com/en/user-guide/snowflake-cortex/snowflake-intelligence)
- [Original Lab Repository](https://github.com/Snowflake-Labs/sfguide-build-data-agents-using-snowflake-cortex-ai)

---

**Happy Learning!** 🚀

For questions or issues, please reach out to your lab instructor.
