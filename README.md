DISCLAIMER: the .yaml files I'm using to run the pipelines were generated using Claude Code, corrected by me and thoroughly tested to make sure they work in different scenarios. However, there might be more efficient ways to do what they are doing. If you notice some possible improvements, let me know and I'll be happy to integrate them.


# DevOps for Power BI: Ensuring Best Practices Compliance

> Automated validation for Power BI Semantic Models and Reports using Tabular Editor BPA and PBI Inspector v2

Code and configuration from the session **"DevOps for Power BI: ensuring best practices compliance with Tabular Editor and PBI Inspector"** presented at Cloud Tech Tallinn 2026.

---

## 📋 What's Included

**Rule Files:**
- `bpa_rules.json` - Standard Tabular Editor BPA rules for semantic models
- `bpa_rules_custom.json` - A few examples of custom BPA rules (naming conventions, TODO checks, duplicates)
- `pbi_insp_v2_standard_rules.json` - PBI Inspector rules for report validation

**Pipeline Files:**
- `run_tebpa_on_modified_pbi_models_standard_rules.yaml` - Validates changed models in PRs using standard rules file
- `run_pbiinsp_v2_on_modified_pbi_reports.yaml` - Validates changed reports in PRs
- `run_tebpa_on_all_pbi_models_standard_rules.yaml*` - Validates all models using standard rules file
- `run_tebpa_on_all_pbi_models_custom_rules.yaml` - Validates all models using custom rules file
- `run_pbiinsp_v2_on_all_pbi_reports.yaml` - Validates all reports

---

## 🚀 Quick Setup

N.B. this part replicates exactly the way it is built in my personal repo. Obviously, you can organize the files in the folder structure you prefer, just make sure to update the variables in the devops library

### Prerequisites
- Azure DevOps project
- Power BI files in `.pbip` format (with `.pbir` version for report files)

### Step 1: Copy Rule Files
Place these in your repository following this structure:

```
your-repo/
├── pbi_devops/
│   ├── te_bpa_rules/
│   │   ├── bpa_rules.json
│   │   └── bpa_rules_custom.json
│   └── pbi_insp_v2_rules/
│       └── pbi_insp_v2_standard_rules.json

```

### Step 2: Create Variable Group
1. Go to **Pipelines → Library** in Azure DevOps
2. Create variable group named `PowerBI-Rules-Paths`
3. Add variables:
   - `te_bpa_standard_rules` = `pbi_devops/pbi_insp_v2_rules/pbi_insp_v2_standard_rules.json`
   - `te_bpa_custom_rules` = `pbi_devops/te_bpa_rules/bpa_rules_custom.json`
   - `pbi_insp_v2_standard_rules` = `pbi_devops/pbi_insp_v2_rules/pbi_insp_v2_standard_rules.json`

### Step 3: Create Pipelines
1. **Pipelines → New pipeline → Existing Azure Pipelines YAML file**
2. Start with these two for PR validation:
   - `run_tebpa_on_modified_pbi_models_standard_rules.yaml`
   - `run_pbiinsp_v2_on_modified_pbi_reports.yaml`

### Step 4: (Optional) Make Mandatory
1. **Repos → Branches → main → Branch policies**
2. Add **Build Validation** for your pipelines
3. Set as **Required**

---

## ⚙️ Customizing Rules

### BPA Rules
Edit `bpa_rules.json` or `bpa_rules_custom.json`:
- Set `"disabled": true` to skip a rule
- Change `"Severity"`: 1=Info, 2=Warning, 3=Error

### PBI Inspector Rules
Edit `pbi_insp_v2_standard_rules.json`:
- Set `"disabled": true` to skip a rule
- Modify thresholds in the `test` section

---

## 💡 Implementation Tips

⚠️ **Start with warnings, not errors!**

1. Deploy with all rules as warnings
2. Fix major violations in existing files
3. Gradually make rules mandatory
4. Get team buy-in before enforcing

---

## 📚 Resources

- 📄 [Session Slides](CTTT_2026_Alessandro_Gherardi_Slide_Deck.pdf)
- 📖 [Tabular Editor BPA Docs](https://docs.tabulareditor.com/te2/Best-Practice-Analyzer.html)
- 🔧 [PBI Inspector V2 GitHub](https://github.com/NatVanG/PBI-InspectorV2)
- 💡 [Rui Romano's DevOps Resources](https://github.com/RuiRomano)
- 🎥 [BPA Video - Daniel Otykier](https://www.youtube.com/watch?v=vJzpJOdPGsE)
- 🎥 [PBI Inspector Video - Nat Van Gulck](https://www.youtube.com/watch?v=_8E0DUzF_l8)

---

## 📧 Contact

**Alessandro Gherardi** | BI Freelancer  
📧 agherardi@agbi.it | 💼 [LinkedIn](https://linkedin.com/in/alessandro-gherardi-07101997/)

---

<div align="center">

⭐ **Star this repo if you found it helpful!** ⭐

*Cloud Tech Tallinn 2026*

</div>
