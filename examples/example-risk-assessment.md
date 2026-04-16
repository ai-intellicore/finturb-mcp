# Example — Risk assessment

**User prompt:**

> *"How risky are markets right now compared to last week?"*

## Tools called

- `get_risk_score` — today's composite read
- `get_risk_history` (days=7 or 14) — comparison window
- `get_signal_strategist` *(optional)* — full dashboard-grade snapshot if the
  user wants more depth

## Condensed response

> Today's composite risk score is **X / 100**, classifying as *<regime>*,
> versus **Y / 100** one week ago (*<prior regime>*). The move is driven
> primarily by *<AR percentile / turbulence percentile / GDELT tone z-score>*,
> which has *<risen / fallen>* from X to Y.
>
> The 4-way liquidity-gated score is **X / 100** (*<regime_4way>*), up/down
> from **Y / 100**. The liquidity boost is +X points today (tight PSI *<yes /
> no>*, tight PLI *<yes / no>*, fragile-shock kicker *<active / inactive>*).
>
> **Week-over-week path (daily composite score):**
>
> ```
> Day -7: Y   regime=A
> Day -6: …   …
> …
> Day  0: X   regime=B   (today)
> ```
>
> Regime duration: the system has been in the current regime for N day(s).
>
> *Informational only — not investment advice.*

## Why this pattern works

The two-tool chain gives Claude both a point-in-time reading and the trajectory
that got us there. The `summary` line on `get_risk_score` is the headline; the
structured body supplies the decomposition if the user asks *"why is it up?"*
The `get_risk_history` response's `summary` already includes first/last dates
and latest regime label — Claude doesn't need to reparse to narrate the
week-over-week comparison.

## Follow-ups

```
What's the probability we stay in this regime versus transitioning in the
  next 10 days?

Is the increase in risk being confirmed by media sentiment, or are the
  signals diverging?

Which assets historically perform best in this regime over the next 21 days?
```
