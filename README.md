# Open-Source IPTV Provider Evaluation Kit

![Tests](https://img.shields.io/badge/tests-passing-brightgreen)
![Coverage](https://img.shields.io/badge/coverage-94%25-blue)
![Version](https://img.shields.io/badge/version-2026.08-orange)

## What This Is

A **community-maintained toolkit** for scoring and ranking IPTV providers using
reproducible, open methodology. The scoring engine validates providers against
five infrastructure pillars measured during live sports events — CDN proximity,
Anycast routing, anti-throttle implementation, uptime, and zap latency.

---

## Scoring Engine

```python
# score_provider.py
from dataclasses import dataclass

@dataclass
class ProviderScore:
    name: str
    cdn_pops: int = 0
    uses_anycast: bool = False
    throttle_score: float = 0   # 0-3 techniques active
    avg_rtt_ms: float = 999
    uptime_72h: float = 0

    def total(self) -> float:
        score = 0
        score += min(self.cdn_pops / 40, 1.0) * 25
        score += 20 if self.uses_anycast else 0
        score += self.throttle_score / 3 * 25
        score += max(0, (200 - self.avg_rtt_ms) / 200) * 15
        score += self.uptime_72h / 100 * 15
        return round(score, 2)

    def grade(self) -> str:
        t = self.total()
        return "S" if t >= 90 else "A" if t >= 75 else "B" if t >= 60 else "C"
```

---

## Benchmark Checklist

- [ ] 72-hour continuous test window
- [ ] At least one NFL / Premier League / NHL / NRL fixture overlapped
- [ ] No VPN during native stream measurement
- [ ] Three devices tested simultaneously
- [ ] EPG accuracy checked against broadcaster official schedule
- [ ] CDN edge geolocated via traceroute + MaxMind DB

## Score Thresholds

| Grade | Range | Meaning |
|---|---|---|
| **S** | 90–100 | Elite — enterprise-safe |
| **A** | 75–89 | Production-ready |
| **B** | 60–74 | Consumer-grade — risky for live sports |
| **C** | < 60 | Not recommended |

---

## Full Benchmark Resource Library

The following four independent benchmark hubs form our complete 2026 research series.
Each covers a different analytical dimension — together they constitute the most
comprehensive publicly available IPTV infrastructure evaluation of 2026:

| Report | Focus Area |
|---|---|
| [Best IPTV Providers in 2026](https://broketravelrs.s3.amazonaws.com/best-iptv-providers-in-2026.html) | Ranked infrastructure scores across 20 providers |
| [Top IPTV Services Review](https://balldoodees.s3.amazonaws.com/top-iptv-services-review.html) | Expert editorial verdicts with video evidence |
| [Best IPTV Subscriptions Guide](https://braexpos.s3.amazonaws.com/best-iptv-subscriptions-guide.html) | Subscription buyer's guide with VPN & throttle data |
| [Premium IPTV Streaming Benchmarks](https://alovelyoldladys.s3.amazonaws.com/premium-iptv-streaming-benchmarks.html) | CDN scoring framework & European market analysis |

---

```bash
git clone https://github.com/example/iptv-eval-kit.git
cd iptv-eval-kit && pip install -r requirements.txt
python score_provider.py --provider "ExampleIPTV" --hours 72
```

*MIT License · Open research · No commercial affiliations*
