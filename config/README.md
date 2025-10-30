# ⚙️ Configuration Files

This folder contains configuration files that participants need to upload to their Snowflake environment.

## 📁 Contents

- **`semantic.yaml`** - Data model definition for Cortex Analyst
- **`semantic_search.yaml`** - Enhanced semantic model with search capabilities  
- **`environment.yml`** - Python dependencies for local development

## 🔧 How to Use These Files

### For Participants

1. **Upload to Snowflake**:
   - Upload `semantic.yaml` and `semantic_search.yaml` to your `@semantic_files` stage
   - Follow the instructions in `../participant-setup/FILE_UPLOAD_GUIDE.md`

2. **Important**: Before uploading, update the schema references:
   - Open both YAML files
   - Replace `USER01` with your actual schema name (e.g., USER02, JOHN_DOE, etc.)
   - This ensures Cortex Analyst can find your tables

### For Local Development (Optional)

- **`environment.yml`** - Use this if you want to run the Streamlit app locally
  ```bash
  conda env create -f environment.yml
  conda activate snowflake-cortex-lab
  ```

## 📋 File Details

### semantic.yaml
- Defines the business data model for sales analytics
- Maps table relationships (DIM_ARTICLE, DIM_CUSTOMER, FACT_SALES)
- Provides business terminology and synonyms
- **Must be customized** with your schema name

### semantic_search.yaml  
- Enhanced version with Cortex Search integration
- Enables dynamic literal retrieval for article names
- Improves natural language query accuracy
- **Must be customized** with your schema name

### environment.yml
- Python package dependencies
- Includes Snowflake connectors and Streamlit
- Only needed for local development

## ⚠️ Important Notes

1. **Schema Customization Required**: Both YAML files contain `schema: USER01` - you MUST change this to your actual schema name
2. **Upload Location**: These files go to your `@semantic_files` stage, not `@docs`
3. **Validation**: After upload, the notebook will verify these files are accessible

## 🔍 Troubleshooting

- **"Schema not found" errors**: Check you updated the schema name in the YAML files
- **"File not found" errors**: Verify files are uploaded to `@semantic_files` stage  
- **Validation errors**: Ensure YAML syntax is correct (no extra spaces/characters)
