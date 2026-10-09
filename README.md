# Social Radar

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-red)
![Power BI](https://img.shields.io/badge/Power_BI-Analytics-yellow)
![Gemini AI](https://img.shields.io/badge/Google_Gemini-AI_Enrichment-orange)
![DuckDB](https://img.shields.io/badge/DuckDB-Database-lightgrey)

## What is Social Radar
Social Radar was created to answer one big question every content creator asks:
**"What should I record next?"** 

Current world revolves around polarizations. Social Radar gathers comments from YouTube, analyzes them by AI, and saves results in CSV files.
Results can be checked in the Streamlit dashboard or for a better view in Power BI.

## Why polarization?
We, humans, have polarization signed in our blood. Starting from tribal wars, through medieval conflicts, up to today's wars on the internet. Social media algorithms brutally use this mechanic to keep people engaged on their platforms. Emotionally hooked people spend a lot more time on the platform, watch more ads, and make the platform more money. That’s why social media rewards videos that provoke discussion and keep audiences engaged.

## How It Works
1. Configuration & Custom Input
   
    -You specify your target niche, search keywords, or specific YouTube video/channel links in the configuration (`.env` or script parameters).
2. Data Ingestion (`run_pipeline.py`)

    -Connects to the **YouTube Data API** to fetch video comments and other data
3. AI Enrichment & Sentiment Analysis (`src/enrichment.py`)

    -Sends comment batches to **Google Gemini (Flash Lite)**.
  
    -Classifies each comment by emotional arousal, specific sentiment, and identifies underlying ideological factions or debate camps.
  
4. Quantitative Indexing & Storage (`src/analytics.py` & `src/db.py`)
   
    -Computes advanced metrics such as **Shannon Polarization Index** (measuring division intensity), **Emotional Arousal** scores, and the proprietary **Topic Opportunity Index (TOI)**.
  
    -Persists all cleaned data, enriched metadata, and analytics into a local **DuckDB** star-schema database for fast querying and CSV exports.
  
5. Interactive Dashboard (`app.py`)

    -Powers a real-time **Streamlit** dashboard featuring interactive Plotly quadrant charts (Polarization vs. Arousal).

    -Highlights trending camps, content gaps, and high-TOI viral opportunities for content creators and researchers.
  
## Power BI

The Power BI report serves as the analytical core of Social Radar, transforming raw data into structured decision making tools

### Key Views(based on mmorpg niche):

* Camp Sentiment & Engagement Overview (Page 1):
 
   * Aggregates comments into high-level thematic clusters.
     
   * Measures physiological emotional intensity and engagement volume.

   * Enables granular exploration of individual comments sorted by engagement.

<img width="1683" height="800" alt="socialradar1" src="https://github.com/user-attachments/assets/1524fdf4-299d-4e0e-a37d-8201f3ba1982" />

 * Unresolved Questions Explorer (Page 2):
 
   * Surfaces unanswered community queries sorted by engagement.
<img width="1683" height="800" alt="socialradar2" src="https://github.com/user-attachments/assets/2d4c0b16-67ed-497f-84e8-9e9096186d6a" />

### Data Modeling & ETL Optimization (Kimball Methodology)
  
Although the upstream Python pipeline extracts 4 raw source tables, the semantic layer was prepared and refined inside **Power Query**:

  * Denormalized and merged `Channels` into `Dim_Videos` to avoid chained relationships, resulting in a clean **Star Schema** with all dimensions connecting directly to `Facts_Comments`.
  * Cleaned encoding issues and resolved inconsistencies across multi-lingual user inputs.
  * Handled unclassified comments by imputing `Neutral` camp instead of allowing `null` values to pollute visual cross-filtering.
    
### Performance & DAX Benefits:

  * Guaranteed 1-to-Many (`1:*`) single-direction relationships with zero circular dependencies.
  * Optimized tabular compression within the Power BI VertiPaq engine.
  * Isolated explicit DAX measures into a dedicated `_Measures` table.
<img width="1683" height="800" alt="socialradar3" src="https://github.com/user-attachments/assets/02dcdb16-8771-469c-802c-3a82132bf850" />

## To-Do List
- Translating non-English comments
- Reddit comments integration
