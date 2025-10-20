# Conversation Log: Campaign Data Migration

**Date**: October 20, 2025  
**Topic**: Campaign Data Migration from CSV to Strapi API  
**Notebook**: `02 - EEP Migrate Campaign.ipynb`

## Summary

Created a comprehensive Jupyter notebook to migrate campaign data from the CSV backup file to the Strapi API database.

## Requirements

- **Source Data**: `data/All Product Data Back Up 070825.csv`
- **Source Columns**:
  - `EEP Campaign Name`
  - `EEP Campaign Launch Date`
- **Target API**: `/api/campaigns` endpoint
- **Field Mapping**:
  - `EEP Campaign Name` → `campaign_name`
  - `EEP Campaign Launch Date` → `campaign_launch_date`

## Implementation Details

### Process Flow

1. Load CSV data (only relevant columns for efficiency)
2. Extract unique campaigns by grouping on campaign name
3. Clean data (remove nulls and empty values)
4. Parse and convert dates to ISO 8601 format (YYYY-MM-DD)
5. Prepare data in Strapi API format
6. Upload campaigns with error handling and rate limiting
7. Log results and generate summary report

### Key Features

- **Date Handling**: Robust date parsing to handle various formats and convert to ISO 8601
- **Error Handling**: Comprehensive error handling for network issues, data inconsistencies, and API errors
- **Logging**: Detailed logging to track progress and issues
- **Rate Limiting**: 0.5 second delay between uploads to avoid overwhelming the API
- **Data Validation**: Removes null/empty campaign names before processing
- **Result Tracking**: Detailed success/failure reporting with campaign names

### Technical Approach

- Used pandas for efficient data manipulation
- Used requests library for API calls
- Implemented proper authentication with Bearer token
- Structured code into reusable functions
- Added comprehensive comments and documentation

## Results Expected

The notebook will:

- Identify all unique campaigns from the source data
- Convert launch dates to proper format
- Upload each campaign to the Strapi database
- Provide detailed summary of successful and failed uploads
- Log all operations for troubleshooting

## Future Considerations

1. **Duplicate Prevention**: Add logic to check for existing campaigns before creating new ones
2. **Update vs Create**: Implement update capability for existing campaigns
3. **Batch Processing**: Explore batch upload if API supports it
4. **Retry Logic**: Add automatic retry for failed uploads
5. **Results Export**: Save upload results to file for audit trail

## Environment Variables Required

- `API_BASE_URL`: Base URL for Strapi API
- `API_TOKEN`: Bearer token for authentication

## Related Files

- Source notebook: `02 - EEP Migrate Campaign.ipynb`
- Previous migration: `EEP Migrate Data.ipynb` (Artist migration)
- Column reference: `00 - Column List.ipynb`
- Schema documentation: `SCHEMA_DOCUMENTATION.md`
