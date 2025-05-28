# Solutions Engineering Workload Modeling - Jupyter Notebook Specification

## Overview
This Jupyter notebook will model Solutions Engineering workload based on revenue targets, conversion rates, staffing levels, and activity assumptions. It will allow scenario modeling to understand capacity requirements and utilization across different business conditions.

## Notebook Structure

### 1. Introduction and Setup
- **Purpose**: Explain the workload modeling approach and its applications
- **Imports**: Import required libraries (pandas, numpy, matplotlib, seaborn, ipywidgets)
- **Helper Functions**: Define calculation functions that will be reused throughout

### 2. Input Parameters Section

#### 2.1 Revenue and Business Targets
**Interactive Widgets:**
- `quarterly_revenue_goal`: Slider widget (0 to 10,000,000, step 50,000, default 2,000,000)
- `average_selling_price`: Slider widget (0 to 1,000,000, step 10,000, default 75,000)

**Explanation**: These parameters drive the required number of new logos needed per quarter.

#### 2.2 Sales Conversion Funnel
**Interactive Widgets:**
- `opportunity_creation_rate`: Slider widget (0 to 100%, step 1%, default 50%)
  - *Percentage of initial meetings that lead to qualified opportunities*
- `demo_conversion_rate`: Slider widget (0 to 100%, step 1%, default 99%)
  - *Percentage of opportunities that result in a demo*
- `tech_eval_conversion_rate`: Slider widget (0 to 100%, step 1%, default 99%)
  - *Percentage of demos that result in a technical evaluation*

#### 2.3 Evaluation Mix and Win Rates
**Interactive Widgets:**
- `self_guided_percentage`: Slider widget (0 to 100%, step 1%, default 65.5%)
- `se_led_percentage`: Slider widget (0 to 100%, step 1%, default 34.5%)
- `rapid_pov_percentage`: Slider widget (0 to 100%, step 1%, default 1%)
- `no_eval_percentage`: Slider widget (0 to 100%, step 1%, default 0%)

*Note: Include validation that these percentages sum to 100%*

**Win Rates by Evaluation Type:**
- `self_guided_win_rate`: Slider widget (0 to 100%, step 1%, default 35%)
- `se_led_win_rate`: Slider widget (0 to 100%, step 1%, default 45%)
- `rapid_pov_win_rate`: Slider widget (0 to 100%, step 1%, default 60%)
- `no_eval_win_rate`: Slider widget (0 to 100%, step 1%, default 10%)

#### 2.4 Activity Time Requirements
**SE Time per Activity (hours):**
- `initial_meeting_time`: Slider widget (0.25 to 2.0, step 0.25, default 0.75)
  - *Including prep time*
- `demo_prep_time`: Slider widget (0.5 to 3.0, step 0.25, default 1.0)
- `demo_delivery_time`: Slider widget (0.5 to 2.0, step 0.25, default 1.0)
- `demo_followup_time`: Slider widget (0.25 to 2.0, step 0.25, default 0.75)
- `self_guided_support_hours_per_week`: Slider widget (0.5 to 5.0, step 0.25, default 2.0)
- `se_led_eval_hours_per_week`: Slider widget (1.0 to 8.0, step 0.5, default 4.0)
- `rapid_pov_total_hours`: Slider widget (10 to 40, step 2, default 30)

#### 2.5 Non-Pipeline Activities
**Fixed Weekly Activities (hours per SE):**
- `slack_email_hours`: Slider widget (2 to 10, step 0.5, default 5.0)
- `troubleshooting_hours`: Slider widget (1 to 8, step 0.5, default 3.75)
- `internal_coordination_hours`: Slider widget (4 to 12, step 0.5, default 7.75)
- `crm_admin_hours`: Slider widget (1 to 5, step 0.25, default 2.5)

### 3. Staffing Configuration

#### 3.1 Team Structure
**Interactive Widgets:**
- `num_ic_ses`: Integer slider (1 to 10, default 1)
  - *Number of full-time Individual Contributor SEs*
- `num_directors`: Integer slider (0 to 5, default 1)
  - *Number of SE Directors*
- `director_ic_percentage`: Slider widget (0 to 100%, step 5%, default 50%)
  - *Percentage of director time spent on IC activities*

#### 3.2 Strategic Activity Allocation
**Time Allocation (hours per week per full-time equivalent SE):**
- `customer_zero_hours_min`: Slider widget (0 to 8, step 0.25, default 2.0)
- `customer_zero_hours_max`: Slider widget (0 to 8, step 0.25, default 4.0)
- `content_creation_hours`: Slider widget (0 to 6, step 0.25, default 2.0)
- `sales_enablement_hours`: Slider widget (0 to 4, step 0.25, default 1.0)
- `developer_advocacy_hours_min`: Slider widget (0 to 4, step 0.25, default 1.0)
- `developer_advocacy_hours_max`: Slider widget (0 to 4, step 0.25, default 2.0)
- `asset_development_hours_avg`: Slider widget (0 to 4, step 0.25, default 1.5)

**Strategic Activity Distribution:**
- `ic_strategic_percentage`: Slider widget (0 to 100%, step 5%, default 70%)
  - *Percentage of strategic activities handled by ICs vs Directors*

### 4. Account Management Configuration

#### 4.1 Account Portfolio
**Account Counts by Segment:**
- `strategic_accounts`: Integer slider (0 to 20, default 3)
- `protect_accounts`: Integer slider (0 to 50, default 7)
- `growth_accounts`: Integer slider (0 to 100, default 24)
- `retain_accounts`: Integer slider (0 to 200, default 59)

#### 4.2 Meeting Frequencies
**Meetings per Month by Segment:**
- `strategic_meetings_per_month`: Slider widget (0.5 to 4.0, step 0.25, default 1.0)
- `protect_meetings_per_month`: Slider widget (0.25 to 2.0, step 0.25, default 1.0)
- `growth_meetings_per_month`: Slider widget (0.25 to 2.0, step 0.25, default 1.0)
- `retain_meetings_per_months`: Slider widget (3 to 24, step 1, default 9)
  - *Every X months (reciprocal used in calculation)*

#### 4.3 New Customer Onboarding
**New Logo Parameters:**
- `new_logos_per_quarter`: Integer slider (0 to 20, default 7)
- `monthly_onboarding_percentage`: Slider widget (0 to 100%, step 5%, default 50%)
  - *Percentage needing monthly meetings for 6 months*
- `quarterly_onboarding_percentage`: Slider widget (0 to 100%, step 5%, default 50%)
  - *Percentage needing quarterly meetings*

#### 4.4 Meeting Time Investment
**Time Parameters:**
- `meeting_duration_hours`: Slider widget (0.25 to 2.0, step 0.25, default 0.5)
- `meeting_followup_hours`: Slider widget (0.25 to 2.0, step 0.25, default 0.625)

### 5. Pipeline Modeling Engine

#### 5.1 Revenue-Driven Calculations
**Code Block**: Calculate required new logos from revenue targets
```python
def calculate_required_new_logos(quarterly_revenue_goal, average_selling_price):
    return quarterly_revenue_goal / average_selling_price
```

#### 5.2 Funnel Back-Calculation
**Code Block**: Work backwards through conversion funnel
```python
def calculate_required_activities(new_logos_needed, conversion_rates, eval_mix, win_rates):
    # Calculate weighted average win rate
    # Work backwards: new logos → evaluations → demos → opportunities → initial meetings
    # Return dict with required weekly activity levels
```

#### 5.3 Evaluation Workload Calculation
**Code Block**: Calculate SE time requirements for different evaluation types
```python
def calculate_evaluation_workload(weekly_evaluations_by_type, time_per_type):
    # Account for duration differences (self-guided ongoing vs rapid POV one-time)
    # Return weekly hours needed
```

### 6. Account Management Modeling

#### 6.1 Meeting Load Calculation
**Code Block**: Calculate total meetings per month
```python
def calculate_account_meetings(account_counts, meeting_frequencies, new_logo_params):
    # Existing account meetings + new logo onboarding meetings
    # Return monthly meeting totals
```

#### 6.2 Account Management Time Requirements
**Code Block**: Convert meetings to SE hours
```python
def calculate_account_mgmt_hours(monthly_meetings, meeting_time_params, num_ses):
    # Factor in meeting duration + follow-up time
    # Distribute across available SEs
```

### 7. Strategic Activities Modeling

#### 7.1 Strategic Time Allocation
**Code Block**: Calculate strategic activity hours per role
```python
def calculate_strategic_workload(strategic_params, staffing_config):
    # Account for IC vs Director distribution
    # Handle min/max ranges for variable activities
    # Return hours per role type
```

### 8. Results Dashboard

#### 8.1 Executive Summary
**Output**: Key metrics in dashboard format
- Revenue target achievement (Yes/No)
- Required new logos per quarter
- Total team utilization percentage
- Capacity surplus/deficit in hours

#### 8.2 Utilization Analysis
**Visualizations**:
- Bar chart: Hours per week by activity category (stacked)
- Gauge charts: Utilization percentage by role
- Table: Detailed time breakdown by SE role

#### 8.3 Pipeline Requirements
**Output Tables**:
- Required weekly activities (meetings, demos, evaluations)
- Conversion funnel flow diagram
- Win rate analysis by evaluation type

#### 8.4 Capacity Analysis
**Visualizations**:
- Waterfall chart showing capacity allocation
- Alert boxes for over-utilization scenarios
- Scenario sensitivity analysis (what-if table)

#### 8.5 Account Management Dashboard
**Output**:
- Meeting load by account segment
- New customer onboarding timeline visualization
- Account management time distribution

### 9. Scenario Comparison (Optional Enhancement)
**Functionality**: Allow users to save current parameter set and compare multiple scenarios side-by-side

## Technical Requirements

### Libraries
- `ipywidgets`: For interactive parameter controls
- `pandas`: Data manipulation and analysis
- `numpy`: Numerical calculations
- `matplotlib` & `seaborn`: Visualization
- `plotly`: Interactive charts (optional for enhanced visualizations)

### Code Style
- **Literate Programming**: Each code block should be preceded by markdown explaining its purpose
- **Modular Functions**: Break calculations into reusable functions
- **Clear Variable Names**: Use descriptive names that match the business context
- **Comments**: Explain business logic and assumptions in code comments
- **Error Handling**: Include basic validation for parameter combinations

### Output Format
- **Professional Appearance**: Clean, business-ready visualizations
- **Clear Labeling**: All charts and tables should have proper titles and axis labels
- **Color Coding**: Consistent color scheme (e.g., red for over-capacity, green for healthy utilization)
- **Responsive Design**: Ensure widgets and outputs display properly in Jupyter

### Validation Rules
- All percentage parameters constrained to 0-100%
- Evaluation type percentages must sum to 100%
- Onboarding percentages must sum to ≤100%
- Warn (but allow) utilization >100%
- Ensure positive values for all time and count parameters

## Usage Flow
1. User opens notebook and runs all cells to initialize
2. User adjusts parameters using interactive widgets
3. Results update automatically (or via button press)
4. User can iterate through different scenarios
5. Final results can be screenshot/printed for external sharing

## Success Criteria
- Notebook produces accurate workload calculations matching manual analysis
- Interactive parameters allow meaningful scenario exploration
- Visualizations clearly communicate capacity constraints and opportunities
- Business users can operate the notebook with minimal technical knowledge