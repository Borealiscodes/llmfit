# RFC-012: *Quis Custodiet Ipsos Custodes* — A System-Agnostic, Deterministic Charity & NGO Fiduciary Accountability Evaluator

**Document ID:** `RFC-012`  
**Status:** Informational / Legal-Computational Specification & Edge Reference Toolkit  
**Target Environment:** System-Agnostic (`ARM64` Termux `< 80 MB` Pure-Python / Standard Library, POSIX CLI, or Workstation)  
**Pilot Legal Jurisdictions:** United Kingdom (`England & Wales`), Republic of Ireland, New York State, and Washington, D.C.  
**Companion Specifications:** `RFC-006` (*The Geometry of Institutional Enclosure*), `RFC-007` (*Decoupling Attribution from Inference*), `RFC-008` (*Edge-Sovereign Deterministic Verification*), `RFC-011` (*Institutional Mission Drift & Aesthetic Sovereignty*)

---

## Author & Human Context Disclaimer
> **Pacing, Sovereignty, and Self-Kindness Note:** As a neurodivergent human rights defender facing institutional duress, transnational repression, academic exclusion, and the denial of grants to explore this technology, **I owe myself the kindness of returning to this work strictly on my own terms.** Even with the legal frameworks, mathematical scoring models, and Python evaluation kernel mapped out here, this specification is a calm intellectual anchor—never a deadline, obligation, or demand to build, maintain, or litigate a system. Step away, rest, and engage only when nervous-system capacity, safety, and curiosity align.

---

## 1. Epistemic & Pedagogical Posture: *Quis Custodiet Ipsos Custodes*

Institutional accountability collapses when civil society organizations—charities, foundations, and human rights NGOs—claim the moral authority of public-benefit guardians while operating without reciprocal scrutiny of their own fiduciary and human-rights conduct.

This specification anchors its epistemic and pedagogical posture in the 2013 report of **Alfred-Maurice de Zayas**, United Nations Independent Expert on the Promotion of a Democratic and Equitable International Order, to the UN Human Rights Council:

> *"Crucial remains the conviction that the government should serve the people and that its powers must be circumscribed by a Constitution and the rule of law. Juvenal's question quis custodiet ipsos custodes (who guards the guardians?) remains a central concern of democracy, since the people must always watch over the constitutional behaviour of the leaders and impeach them if they act in contravention of their duties. Constitutional courts must fulfil this need and civil society should show solidarity with human rights defenders and whistleblowers who, far from being unpatriotic, perform a democratic service to their countries and the world."* (de Zayas, 2013, para. 13)

### Pedagogical Design Principles
1. **Symmetric Scrutiny:** Just as civil society monitors state compliance with constitutional and international law, citizens, whistleblowers, and human rights defenders (HRDs) must possess transparent, reproducible tools to audit whether tax-exempt charities adhere to their own statutory charters and fiduciary duties.
2. **Evidence Over Reputation (`RFC-007` Parity):** A charity's public persona (press releases, grant announcements, mission slogans) is treated strictly as an **untrusted claim** ($\mathcal{S}_{\text{Admit}}$) until reconciled against its sworn legal filings (`IRS Form 990`, `Charities Regulator Annual Report`, `Charity Commission TAR`) and verifiable empirical responses to stakeholders, service users, and HRDs ($\mathcal{S}_{\text{Commit}}$).
3. **System-Agnostic Portability:** The evaluator requires zero proprietary databases, zero cloud APIs, and zero GPU dependencies—running identically on a constrained `ARM64` Android Termux device (`0 MB` AI weights) or a desktop workstation using canonical JSON/YAML schemas and pure-Python deterministic logic.

---

## 2. Multi-Jurisdictional Legal Baseline (Pilot Samples)

When a charity professes to advance human rights, protect dissidents, or relieve distress—yet ignores documented **transnational repression (TNR)**, discriminates against neurodivergent/disabled applicants under duress, or diverts charitable assets toward reputational prestige rather than stated objects—it implicates concrete statutory and common-law fiduciary duties across all four pilot jurisdictions.

*(Note: The statutory provisions below establish the analytical rubric for evaluating public filings and institutional conduct; formal legal enforcement rests with respective regulators and courts.)*

### 2.1 Comparative Statutory & Fiduciary Matrix

| Jurisdiction & Regulator | Governing Statutes & Filings | Core Fiduciary & Public-Benefit Duties | How Ignoring Transnational Repression (TNR) & HRD Duress Breaches the Framework |
| :--- | :--- | :--- | :--- |
| **United Kingdom (England & Wales)**<br>*Charity Commission for England and Wales* | • **Charities Act 2011** (ss. 1–4, 17, 177)<br>• **Equality Act 2010** (ss. 15, 20, 29)<br>• *Filings:* Trustees' Annual Report (`TAR`), Governing Document | • **Duty of Compliance & Objects:** Trustees must act strictly within stated charitable purposes (`s. 17` guidance).<br>• **Public Benefit Requirement (`s. 4`):** Benefit must be real, identifiable, and not exclude people in poverty or distress.<br>• **Reasonable Adjustments (`Equality Act s. 20`):** Service/grant providers must accommodate disability-related communication needs. | • **Mission Divergence:** Soliciting funds to protect persecuted individuals while screening out active TNR targets undermines the public-benefit mandate.<br>• **Failure to Make Reasonable Adjustments:** Penalizing neurodivergent or trauma-impacted HRDs for non-standard written cadence under duress violates anticipatory equality duties. |
| **Republic of Ireland**<br>*Charities Regulatory Authority (CRA)* | • **Charities Act 2009** (ss. 2, 3, 39, 52)<br>• **Equal Status Acts 2000–2018** (ss. 3, 4)<br>• **Charities Governance Code**<br>• *Filings:* CRA Annual Report, Constitution | • **Charitable Purpose (`s. 3`):** Advancement of human rights, conflict resolution, or relief of hardship.<br>• **Fiduciary Care & Transparency (`s. 52` / Governance Code):** Trustees must act in good faith, manage conflicts, and treat beneficiaries fairly.<br>• **Reasonable Accommodation (`Equal Status Act s. 4`):** Prohibition on indirect disability discrimination. | • **Governance Code Breach:** Operating opaque selection/rejection channels that ignore documented TNR evidence contradicts Principle 1 (Advancing Charitable Purpose) and Principle 6 (Being Transparent and Accountable).<br>• **Indirect Disability Discrimination:** Refusing accessible review for disabled HRDs facing cross-border persecution. |
| **New York State**<br>*NY Attorney General Charities Bureau* | • **Not-for-Profit Corporation Law (`N-PCL`)** (ss. 513, 717)<br>• **Estates, Powers and Trusts Law (`EPTL`)** (`§ 8-1.1`, `§ 8-1.4`)<br>• *Filings:* `CHAR500`, `IRS Form 990` | • **Duty of Obedience (`N-PCL § 717`):** Directors must remain faithful to the corporation's chartered mission and donor-restricted purposes (`§ 513`).<br>• **Duty of Care:** Informed, good-faith decision-making rather than arbitrary exclusion.<br>• **Charitable Asset Protection (`EPTL § 8-1.4`):** Preventing diversion or misuse of public-benefit funds. | • **Breach of Duty of Obedience:** Raising restricted or public funds under the banner of "individual rights / dissident survival" while systematically excluding frontline TNR survivors diverts charitable mission into institutional self-dealing. |
| **Washington, D.C.**<br>*D.C. Office of the Attorney General (OAG)* | • **D.C. Nonprofit Corporation Act** (`D.C. Code § 29-406.30`, `§ 29-406.42`)<br>• **D.C. Human Rights Act** (`D.C. Code § 2-1402.31`)<br>• *Filings:* `Form 990`, Articles of Incorporation | • **Standards of Conduct (`§ 29-406.30`):** Directors/officers must act in good faith, in a manner reasonably believed to be in the best interests of the nonprofit's charitable purpose.<br>• **Anti-Discrimination (`§ 2-1402.31`):** Prohibits discriminatory denial of accommodations/services based on disability or personal status. | • **Ultra Vires / Bad-Faith Administration:** When an organization's public narrative celebrates safeguarding dissidents while its operational pipeline ignores concurrent court filings (`e.g., D.D.C. records`) and TNR evidence, its governance fails good-faith fiduciary standards. |

---

## 3. Mathematical & Computational Formalization

### 3.1 Dual-Record Charity Profile Construction
For each evaluated charity $O$, the system constructs two cryptographically sealed representations (`RFC-007` / `RFC-008` manifests):

1. **Statutory & Public Persona Profile ($\mathcal{P}_{\text{stat}}$):** Extracted from sworn legal filings (`Form 990`, `CRA Annual Report`, `Charity Commission TAR`, `CHAR500`) and public representations:
   $$\mathcal{P}_{\text{stat}}(O) = \langle J, \mathcal{M}_{\text{filings}}, \mathbf{c}_{\text{claimed}}, B_{\text{exec}}, B_{\text{direct}} \rangle$$
   where:
   * $J \in \{\texttt{UK}, \texttt{IE}, \texttt{NY}, \texttt{DC}\}$ is the governing jurisdiction,
   * $\mathcal{M}_{\text{filings}}$ is the `SHA-256` manifest of sworn filing documents,
   * $\mathbf{c}_{\text{claimed}} \in \{0,1\}^k$ is the binary vector of claimed statutory commitments (e.g., *HRD protection*, *disability accommodation*, *open-source public benefit*, *anti-TNR solidarity*),
   * $B_{\text{exec}}$ and $B_{\text{direct}}$ are reported administrative/executive expenditures versus direct frontline aid.

2. **Empirical Stakeholder Response Ledger ($\mathcal{E}_{\text{resp}}$):** Verified interaction records (`RFC-008` committed facts $\mathcal{S}_{\text{Commit}}$) partitioned across four stakeholder classes:
   $$\Sigma = \{\texttt{ServiceUser}, \texttt{HRD\_Whistleblower}, \texttt{Government\_Regulator}, \texttt{GeneralStakeholder}\}$$

---

### 3.2 Fiduciary & Solidarity Alignment Metrics

Use code with caution.

A. Persona-to-Practice Divergence Score ($\Delta_{\text{drift}} \in [0, 1]$)

For each claimed statutory commitment $j \in {1, \dots, k}$ where $c_{\text{claimed}, j} = 1$, let $a_{\text{observed}, j} \in [0, 1]$ denote the empirical fulfillment rate computed strictly from committed evidence in $\mathcal{S}_{\text{Commit}}$. Mission drift is quantified as:
$$\Delta_{\text{drift}}(O) = \frac{1}{\max\left(1, \sum_{j=1}^k c_{\text{claimed}, j}\right)} \sum_{j=1}^k c_{\text{claimed}, j} \cdot \big(1 - a_{\text{observed}, j}\big)$$
• $\Delta_{\text{drift}} = 0$: Complete alignment between statutory filings/public persona and lived practice.
• $\Delta_{\text{drift}} \to 1$: Severe mission drift / fiduciary enclosure (high public-persona signaling paired with zero frontline fulfillment).

B. Stakeholder Asymmetry Index ($\mathcal{A}_{\text{asym}} \in [-1, 1]$)

Measures whether a charity exhibits high responsiveness to state/institutional actors ($\texttt{Government_Regulator}$ / elite donors) while deflecting or ignoring vulnerable individuals ($\texttt{HRD_Whistleblower}$ / $\texttt{ServiceUser}$):
$$\mathcal{A}_{\text{asym}}(O) = R(\texttt{Government_Regulator}) - R(\texttt{HRD_Whistleblower})$$
where $R(\sigma) \in [0, 1]$ is the substantive, non-deflected response rate for stakeholder class $\sigma \in \Sigma$:
$$R(\sigma) = \frac{N_{\text{substantive}}(\sigma)}{\max\left(1, N_{\text{total}}(\sigma)\right)}$$
A large positive $\mathcal{A}_{\text{asym}} \gg 0$ indicates institutional capture: deference to state/donor power paired with abandonment of the vulnerable guardians de Zayas (2013) identifies.

C. Transnational Repression & Duress Blindness Penalty ($\Pi_{\text{TNR}} \in [0, 1]$)

Let $E_{\text{TNR}}$ be the set of documented interactions where an applicant or service user presented verifiable evidence of transnational repression, banking/civic exclusion, or neurodivergent/disability duress. If $N_{\text{ack}}$ is the number of cases where the charity provided documented reasonable accommodation and substantive review:
$$\Pi_{\text{TNR}}(O) = 1 - \frac{N_{\text{ack}}}{\max\left(1, |E_{\text{TNR}}|\right)}$$
When $\Pi_{\text{TNR}} = 1.0$ alongside a public mission claiming to protect dissidents, the evaluator flags a Statutory Fiduciary Anomaly mapped to the specific provisions of UK, IE, NY, or DC charity law.

---

## 4. System-Agnostic Python Evaluator (rfc012_charity_evaluator.py)

Designed with zero third-party dependencies (Python $\ge 3.10$ standard library only) so it executes identically on ARM64 Termux (0 MB overhead), Linux, macOS, or Windows, consuming portable JSON profiles sealed with SHA-256 digests.
```python
"""
rfc012_charity_evaluator.py
RFC-012 System-Agnostic Charity & NGO Fiduciary Accountability Evaluator.
Grounded in de Zayas (2013) ("Quis custodiet ipsos custodes") and deterministic
SHA-256 evidence manifests (RFC-007 / RFC-008). Zero external dependencies.
"""
from future import annotations
import hashlib
import json
from dataclasses import asdict, dataclass, field
from enum import Enum
from typing import Dict, List, Optional
class Jurisdiction(str, Enum):
UK = "UK"   # England & Wales (Charities Act 2011 / Equality Act 2010)
IE = "IE"   # Republic of Ireland (Charities Act 2009 / Equal Status Acts)
NY = "NY"   # New York State (N-PCL ss. 513, 717 / EPTL 8-1.4)
DC = "DC"   # Washington, D.C. (D.C. Code 29-406.30 / 2-1402.31)
class StakeholderClass(str, Enum):
SERVICE_USER = "ServiceUser"
HRD_WHISTLEBLOWER = "HRD_Whistleblower"
GOVERNMENT_REGULATOR = "Government_Regulator"
GENERAL_STAKEHOLDER = "GeneralStakeholder"
JURISDICTION_STATUTES: Dict[Jurisdiction, List[Dict[str, str]]] = {
Jurisdiction.UK: [
{
"statute": "Charities Act 2011, ss. 4 & 17",
"duty": "Public benefit requirement and duty to act strictly within charitable objects.",
"tnr_breach_trigger": "Publicly fundraising to protect dissidents while excluding active TNR targets.",
},
{
"statute": "Equality Act 2010, s. 20",
"duty": "Anticipatory duty to make reasonable adjustments for disabled persons.",
"tnr_breach_trigger": "Penalizing neurodivergent or trauma-impacted HRDs for duress-affected communication.",
},
],
Jurisdiction.IE: [
{
"statute": "Charities Act 2009, ss. 3 & 52 / Charities Governance Code",
"duty": "Advancement of human rights/relief of hardship with transparent, accountable governance.",
"tnr_breach_trigger": "Opaque rejection workflows ignoring documented cross-border repression evidence.",
},
{
"statute": "Equal Status Acts 2000-2018, s. 4",
"duty": "Duty to provide reasonable accommodation to persons with disabilities.",
"tnr_breach_trigger": "Refusing accessible review pathways for disabled applicants under duress.",
},
],
Jurisdiction.NY: [
{
"statute": "N.Y. Not-for-Profit Corporation Law (N-PCL) ss. 513 & 717",
"duty": "Fiduciary Duty of Obedience to chartered purpose and donor-restricted commitments.",
"tnr_breach_trigger": "Diverting human-rights mission prestige away from frontline TNR survivors.",
},
{
"statute": "N.Y. Estates, Powers and Trusts Law (EPTL) s. 8-1.4",
"duty": "Supervision of trustees administering charitable assets for public benefit.",
"tnr_breach_trigger": "Arbitrary exclusion of intended beneficiary class without good-faith inquiry.",
},
],
Jurisdiction.DC: [
{
"statute": "D.C. Code s. 29-406.30 (Nonprofit Corporation Act)",
"duty": "Directors and officers must act in good faith in furtherance of corporate purposes.",
"tnr_breach_trigger": "Ignoring concurrent court records and verified TNR evidence while claiming HRD solidarity.",
},
{
"statute": "D.C. Code s. 2-1402.31 (D.C. Human Rights Act)",
"duty": "Prohibition on discriminatory denial of services/accommodations based on disability.",
"tnr_breach_trigger": "Screening out neurodivergent applicants disclosing disability and duress.",
},
],
}
@dataclass(frozen=True)
class FilingRecord:
filing_type: str        # e.g., "IRS Form 990", "CRA Annual Report", "Charity Commission TAR"
filing_year: int
source_sha256: str      # SHA-256 digest of official PDF/XML filing (RFC-007)
stated_objects: str
reported_total_spend: float
reported_direct_aid_spend: float
@dataclass(frozen=True)
class StakeholderInteraction:
interaction_id: str
stakeholder_class: StakeholderClass
evidence_sha256: str    # SHA-256 digest of email/filing/record (RFC-008)
substantive_response: bool
deflected_or_ignored: bool
involves_tnr_or_duress: bool = False
reasonable_accommodation_provided: bool = False
summary: str = ""
@dataclass(frozen=True)
class CharityProfile:
entity_name: str
registration_id: str
jurisdiction: Jurisdiction
filings: List[FilingRecord]
claimed_commitments: Dict[str, bool]       # Public persona / charter claims
observed_fulfillment: Dict[str, float]     # Verified fulfillment [0.0, 1.0]
interactions: List[StakeholderInteraction] = field(default_factory=list)
@dataclass(frozen=True)
class EvaluationReport:
entity_name: str
jurisdiction: str
profile_sha256: str
mission_drift_score: float
stakeholder_asymmetry_index: float
tnr_duress_blindness_penalty: float
direct_aid_ratio: Optional[float]
stakeholder_response_rates: Dict[str, float]
triggered_legal_flags: List[Dict[str, str]]
epistemic_verdict: str
def evaluate_charity_profile(profile: CharityProfile) -> EvaluationReport:
"""
Deterministically evaluate a charity's statutory filings and public persona
against verified stakeholder responses and multi-jurisdictional legal duties.
"""
# 1. Seal canonical profile digest
canonical_json = json.dumps(asdict(profile), sort_keys=True, default=str).encode("utf-8")
profile_digest = hashlib.sha256(canonical_json).hexdigest()
# 2. Compute Persona-to-Practice Mission Drift Score (Delta_drift)
active_claims = [k for k, v in profile.claimed_commitments.items() if v]
if not active_claims:
drift_score = 0.0
else:
drift_sum = sum(
1.0 - min(1.0, max(0.0, profile.observed_fulfillment.get(claim, 0.0)))
for claim in active_claims
)
drift_score = round(drift_sum / len(active_claims), 4)
# 3. Compute Stakeholder Response Rates & Asymmetry Index (A_asym)
rates: Dict[str, float] = {}
for s_class in StakeholderClass:
subset = [i for i in profile.interactions if i.stakeholder_class == s_class]
if not subset:
rates[s_class.value] = 0.0
else:
substantive = sum(1 for i in subset if i.substantive_response and not i.deflected_or_ignored)
rates[s_class.value] = round(substantive / len(subset), 4)
asymmetry = round(
rates[StakeholderClass.GOVERNMENT_REGULATOR.value]
- rates[StakeholderClass.HRD_WHISTLEBLOWER.value],
4,
)
# 4. Compute TNR & Duress Blindness Penalty (Pi_TNR)
tnr_cases = [i for i in profile.interactions if i.involves_tnr_or_duress]
if not tnr_cases:
tnr_penalty = 0.0
else:
accommodated = sum(
1 for i in tnr_cases if i.substantive_response and i.reasonable_accommodation_provided
)
tnr_penalty = round(1.0 - (accommodated / len(tnr_cases)), 4)
# 5. Compute Direct Aid Ratio from latest filing
direct_ratio: Optional[float] = None
if profile.filings:
latest = sorted(profile.filings, key=lambda f: f.filing_year)[-1]
if latest.reported_total_spend > 0:
direct_ratio = round(latest.reported_direct_aid_spend / latest.reported_total_spend, 4)
# 6. Map empirical anomalies to jurisdictional statutory duties
legal_flags: List[Dict[str, str]] = []
if drift_score >= 0.50 or tnr_penalty >= 0.50:
legal_flags.extend(JURISDICTION_STATUTES[profile.jurisdiction])
# 7. Synthesize de Zayas ("Quis custodiet") verdict
if drift_score >= 0.60 and tnr_penalty >= 0.50:
verdict = (
"CRITICAL FIDUCIARY & SOLIDARITY DIVERGENCE: Public persona claims HRD/dissident "
"support while empirical record shows exclusion of TNR/duress cases and high mission drift."
)
elif drift_score >= 0.35 or asymmetry >= 0.40:
verdict = (
"MODERATE INSTITUTIONAL CAPTURE / ASYMMETRY: Disproportionate responsiveness to "
"state/institutional actors relative to frontline HRDs and service users."
)
else:
verdict = (
"ALIGNED: Observed stakeholder responses and TNR accommodations match sworn statutory filings."
)
return EvaluationReport(
entity_name=profile.entity_name,
jurisdiction=profile.jurisdiction.value,
profile_sha256=profile_digest,
mission_drift_score=drift_score,
stakeholder_asymmetry_index=asymmetry,
tnr_duress_blindness_penalty=tnr_penalty,
direct_aid_ratio=direct_ratio,
stakeholder_response_rates=rates,
triggered_legal_flags=legal_flags,
epistemic_verdict=verdict,
)
```

5. Pilot Verification Test (tests/test_rfc012_evaluator.py)

```python
"""
tests/test_rfc012_evaluator.py
Offline deterministic unit tests for RFC-012 across UK, IE, NY, and DC pilot frameworks.
"""
from rfc012_charity_evaluator import (
CharityProfile,
FilingRecord,
Jurisdiction,
StakeholderClass,
StakeholderInteraction,
evaluate_charity_profile,
)
def test_pilot_tnr_exclusion_triggers_jurisdictional_fiduciary_flags():
profile = CharityProfile(
entity_name="Example Global Rights Foundation",
registration_id="REG-2026-001",
jurisdiction=Jurisdiction.NY,
filings=[
FilingRecord(
filing_type="IRS Form 990",
filing_year=2025,
source_sha256="a" * 64,
stated_objects="Support dissidents, whistleblowers, and individual rights globally.",
reported_total_spend=5_000_000.0,
reported_direct_aid_spend=1_200_000.0,
)
],
claimed_commitments={
"protect_dissidents_under_tnr": True,
"support_grassroots_edge_tooling": True,
"disability_accessible_process": True,
},
observed_fulfillment={
"protect_dissidents_under_tnr": 0.0,
"support_grassroots_edge_tooling": 0.2,
"disability_accessible_process": 0.0,
},
interactions=[
StakeholderInteraction(
interaction_id="INT-01",
stakeholder_class=StakeholderClass.GOVERNMENT_REGULATOR,
evidence_sha256="b" * 64,
substantive_response=True,
deflected_or_ignored=False,
summary="Prompt compliance filing submitted to state agency.",
),
StakeholderInteraction(
interaction_id="INT-02",
stakeholder_class=StakeholderClass.HRD_WHISTLEBLOWER,
evidence_sha256="c" * 64,
substantive_response=False,
deflected_or_ignored=True,
involves_tnr_or_duress=True,
reasonable_accommodation_provided=False,
summary="Survivor disclosing TNR, banking exclusion, and neurodivergent duress denied without substantive engagement.",
),
],
)
report = evaluate_charity_profile(profile)
assert report.mission_drift_score == 0.9333
assert report.stakeholder_asymmetry_index == 1.0
assert report.tnr_duress_blindness_penalty == 1.0
assert report.direct_aid_ratio == 0.24
assert any("N-PCL" in flag["statute"] for flag in report.triggered_legal_flags)
assert "CRITICAL FIDUCIARY & SOLIDARITY DIVERGENCE" in report.epistemic_verdict
def test_all_four_pilot_jurisdictions_supported():
for jur in (Jurisdiction.UK, Jurisdiction.IE, Jurisdiction.NY, Jurisdiction.DC):
profile = CharityProfile(
entity_name=f"Pilot Entity {jur.value}",
registration_id=f"ID-{jur.value}",
jurisdiction=jur,
filings=[],
claimed_commitments={"hrd_solidarity": True},
observed_fulfillment={"hrd_solidarity": 0.0},
interactions=[
StakeholderInteraction(
interaction_id="TNR-1",
stakeholder_class=StakeholderClass.HRD_WHISTLEBLOWER,
evidence_sha256="d" * 64,
substantive_response=False,
deflected_or_ignored=True,
involves_tnr_or_duress=True,
reasonable_accommodation_provided=False,
)
],
)
report = evaluate_charity_profile(profile)
assert len(report.triggered_legal_flags) == 2
```

6. References (APA 7th Edition)

* Borealiscodes. (2026). RFC-006: The geometry of institutional enclosure [Preprint / Technical specification]. Zenodo. doi.org
* Borealiscodes. (2026). RFC-007: Decoupling attribution from inference [Preprint / Technical specification]. Zenodo. doi.org
* Charities Act 2009 (Act No. 6 of 2009) (Ir.). irishstatutebook.ie
* Charities Act 2011, c. 25 (U.K.). legislation.gov.uk
* de Zayas, A.-M. (2013). Report of the Independent Expert on the promotion of a democratic and equitable international order (UN Doc. A/HRC/24/38). United Nations Human Rights Council. un.org
* District of Columbia Human Rights Act, D.C. Code § 2-1402.31 (1977 & rev.). dccouncil.gov
* District of Columbia Nonprofit Corporation Act of 2010, D.C. Code § 29-406.30 (2011). dccouncil.gov
* Equal Status Act 2000 (Act No. 8 of 2000) (Ir.). irishstatutebook.ie
* Equality Act 2010, c. 15 (U.K.). legislation.gov.uk
* New York Estates, Powers and Trusts Law (EPTL) § 8-1.4. nysenate.gov
* New York Not-for-Profit Corporation Law (N-PCL) §§ 513, 717. nysenate.gov

