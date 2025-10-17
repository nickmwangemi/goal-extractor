# Goal Extractor Dashboard

A comprehensive Streamlit dashboard for analyzing mastermind group goal tracking and accountability data.

## Features

### 📊 Dashboard Tabs

1. **🚨 Vague Goals** - QA queue for goals needing clarification
2. **🎯 Quantifiable Goals** - Track progress on measurable goals
3. **📢 Community Posting** - Monitor Slack/Discord goal posts
4. **📊 Attendance & Achievement** - Member attendance and goal completion rates
5. **📝 Member Changes** - Track member lifecycle events (group changes, renewals, pauses)
6. **📈 Marketing Activities** - Extract and analyze marketing activities
7. **🧠 Challenges & Strategies** - Identify challenges and shared strategies
8. **🚨 Group Health** - Monitor stuck signals, help offers, and sentiment
9. **📊 Source Tracking** - Analytics on goal update sources (AI vs human input)

## Setup

### 1. Install Dependencies

```bash
pip install -r dashboard_requirements.txt
```

### 2. Environment Variables

Create a `.env` file with your Supabase credentials:

```bash
# Supabase Configuration
SUPABASE_URL=your_supabase_project_url_here
SUPABASE_ANON_KEY=your_supabase_anon_key_here

# Optional: Organization ID for filtering data
ORGANIZATION_ID=your_organization_id_here
```

### 3. Run the Dashboard

```bash
streamlit run dashboard.py
```

The dashboard will open in your browser at `http://localhost:8501`

## Usage

### Data Sources

The dashboard is **completely decoupled** from the main processing script and queries Supabase directly for:

- `vague_goals_detected` - Goals needing clarification
- `quantifiable_goals` - Trackable goals with numbers
- `community_posts` - Posted goals to Slack/Discord
- `member_attendance` - Session attendance tracking
- `member_change_log` - Member lifecycle events
- `marketing_activities` - Marketing activity extraction
- `challenges` & `strategies` - Challenge and strategy data
- `stuck_signals` & `help_offers` - Group health indicators
- `call_sentiment` - Sentiment analysis data

### Key Features

#### 🚨 Vague Goals Tab
- View goals that need QA team clarification
- Filter by status, participant, or date
- Mark goals as clarified
- Track QA team workflow

#### 🎯 Quantifiable Goals Tab
- Track goal progress with visual indicators
- Filter by participant or source type
- Update progress manually
- View goal source analytics

#### 📊 Source Tracking Tab
- Analyze goal update sources (AI vs human)
- Track QA team contributions
- Monitor data quality metrics
- View top goal updaters

#### 🚨 Group Health Tab
- Monitor sentiment scores over time
- Identify stuck participants
- Track help offers and support
- Generate health alerts

### Caching

The dashboard uses Streamlit's caching to optimize performance:
- Data is cached for 5 minutes (`@st.cache_data(ttl=300)`)
- Supabase connection is cached (`@st.cache_resource`)
- Refresh data by clicking the refresh button or waiting for cache expiry

### Real-time Updates

- Click refresh buttons to update data
- Use filters to narrow down results
- Interactive charts with Plotly
- Expandable sections for detailed views

## Architecture

### Decoupled Design

The dashboard is intentionally decoupled from the main processing script:

```
main.py (Processing) → Supabase Database ← dashboard.py (Analytics)
```

This allows:
- Independent deployment and scaling
- Real-time data access without processing overhead
- Multiple dashboard instances
- Easy maintenance and updates

### Data Flow

1. **Processing**: `main.py` processes transcripts and stores data in Supabase
2. **Analytics**: `dashboard.py` queries Supabase for real-time analytics
3. **Visualization**: Streamlit renders interactive charts and tables
4. **Interaction**: Users can filter, update, and analyze data

## Customization

### Adding New Tabs

1. Create a new tab function following the pattern:
```python
def show_new_feature_tab(supabase: Client):
    st.header("🆕 New Feature")
    # Implementation here
```

2. Add the tab to the main function:
```python
with st.tabs([..., "🆕 New Feature"]):
    show_new_feature_tab(supabase)
```

### Modifying Data Sources

Update the fetch functions to query different tables or add new filters:

```python
@st.cache_data(ttl=300)
def fetch_custom_data(supabase: Client, organization_id: str = None):
    # Custom query implementation
    pass
```

### Styling

The dashboard uses Streamlit's built-in styling with:
- Wide layout for better data visualization
- Icons and emojis for visual appeal
- Consistent color schemes
- Responsive design

## Troubleshooting

### Common Issues

1. **Supabase Connection Error**
   - Check environment variables
   - Verify Supabase URL and key
   - Ensure database tables exist

2. **No Data Showing**
   - Check if data exists in Supabase
   - Verify table names match database schema
   - Check organization_id filter

3. **Performance Issues**
   - Data is cached for 5 minutes
   - Use filters to reduce data load
   - Consider pagination for large datasets

### Support

For issues or questions:
1. Check the Supabase connection
2. Verify data exists in the database
3. Review the console logs for errors
4. Check environment variable configuration
