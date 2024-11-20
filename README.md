def calculate_lead_score(company_size, annual_budget, industry, urgency):
    score = 0

    # Company Size Scoring
    if company_size <= 50:
        score += 10
    elif company_size <= 200:
        score += 20
    elif company_size <= 1000:
        score += 30
    else:
        score += 40

    # Annual Budget Scoring
    if annual_budget < 10000:
        score += 5
    elif annual_budget <= 50000:
        score += 20
    elif annual_budget <= 100000:
        score += 35
    else:
        score += 50

    # Industry Scoring
    industry_scores = {
        "Technology": 30,
        "Finance": 25,
        "Healthcare": 20,
        "Retail": 15,
        "Other": 10
    }
    score += industry_scores.get(industry, 10)  # Default to 10 if industry not found

    # Urgency Scoring
    if urgency == "Immediate":
        score += 40
    elif urgency == "Short-term":
        score += 30
    elif urgency == "Medium-term":
        score += 20
    else:
        score += 10

    return score

# Example usage
company_size = 150  # Example input
annual_budget = 30000  # Example input
industry = "Technology"  # Example input
urgency = "Immediate"  # Example input

lead_score = calculate_lead_score(company_size, annual_budget, industry, urgency)
print("Lead Score:", lead_score)

# Action based on score
if lead_score >= 70:
    # Add to "Qualified Leads" Google Sheet
    pass
else:
    # Add to "Nurture Campaign" Google Sheet
    pass

