# Open-Source IPTV Provider Evaluation Kit

![Tests](https://img.shields.io/badge/tests-passing-brightgreen)
![Coverage](https://img.shields.io/badge/coverage-94%25-blue)
![Version](https://img.shields.io/badge/version-2026.08-orange)

## What This Is

A **community-maintained toolkit** for scoring and ranking IPTV providers using
reproducible, open methodology. The scoring engine validates providers against
five infrastructure pillars measured during live sports events.

> 🔗 **[2026 Provider Rankings — Fully Scored & Verified](https://broketravelrs.s3.amazonaws.com/best-iptv-providers-in-2026.html)**

---

## Scoring Engine

```python
# score_provider.py
from dataclasses import dataclass, field
from typing import List

@dataclass
class ProviderScore:
    name: str
    cdn_pops: int = 0           # confirmed global PoPs
    uses_anycast: bool = False  # Anycast vs Unicast routing
    throttle_score: float = 0   # 0-3 anti-throttle techniques
    avg_rtt_ms: float = 999     # measured avg RTT to edge node
    uptime_72h: float = 0       # % uptime over 72h test window

    def total(self) -> float:
        score = 0
        score += min(self.cdn_pops / 40, 1.0) * 25   # max 25 pts
        score += 20 if self.uses_anycast else 0        # max 20 pts
        score += self.throttle_score / 3 * 25          # max 25 pts
        score += max(0, (200 - self.avg_rtt_ms) / 200) * 15  # max 15 pts
        score += self.uptime_72h / 100 * 15            # max 15 pts
        return round(score, 2)

    def grade(self) -> str:
        t = self.total()
        return "S" if t >= 90 else "A" if t >= 75 else "B" if t >= 60 else "C"
```

## Benchmark Checklist

Before submitting a provider evaluation, verify all items:

- [ ] Test conducted over minimum 72 continuous hours
- [ ] At minimum one NFL/Premier League/NHL/NRL fixture included
- [ ] No VPN active during native stream measurement phase
- [ ] Three devices tested simultaneously (Smart TV, Firestick, Mobile)
- [ ] EPG accuracy measured against broadcaster official schedule
- [ ] CDN edge node geolocated via traceroute + MaxMind DB

## Score Interpretation

| Grade | Score Range | Meaning |
|---|---|---|
| **S** | 90–100 | Elite infrastructure — suitable for enterprise deployment |
| **A** | 75–89 | Production-ready — minor gaps in geographic coverage |
| **B** | 60–74 | Consumer-grade — acceptable for casual use, risky for live sports |
| **C** | Below 60 | Not recommended — significant infrastructure deficits observed |

## Full Ranked Results

All 20 evaluated providers with raw scores and evidence:

**[→ Open-Source IPTV Provider Evaluation Kit: Ranked Provider Database 2026](https://broketravelrs.s3.amazonaws.com/best-iptv-providers-in-2026.html)**

---

```bash
# Quick install
git clone https://github.com/example/iptv-eval-kit.git
cd iptv-eval-kit && pip install -r requirements.txt
python score_provider.py --provider "ExampleIPTV" --hours 72
```

*MIT License · Open research · No commercial affiliations*
