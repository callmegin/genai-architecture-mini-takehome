# Candidate Response

> **Instructions:** Complete all sections below. Keep your total response to ~500 words (one page max).
>
> **Time limit:** 20 minutes recommended, 30 minutes hard stop.

---

## Your Name

*[Enter your name here]*

---

## Section A: Top 5 Risks (Ranked)

*Identify the five most critical risks in the proposed design. Rank them from highest to lowest priority. For each risk, provide a brief description (1-2 sentences).*

1. No proper RBAC control

   Even though the role is available in the token, the design doc does not mention it being used, nor it's being shown in system overview graph.
   Allowing users to access / see data which should be blocked.

2. Admin endpoint and JWT
   
   No mention that admin endpoint is properly secured. Especially when JWT is not actively revoked. This brings up potential issues when someone's role is changed. I.e. downgrading it.

3. LLM hallucination with non-existing customer/user data

   *"Fallback Behavior: If the LLM cannot find relevant data, it will estimate reasonable values based on industry benchmarks to ensure the user always receives an answer."*
   This line implies that LLM can return non-existent data, especially crucial when we're talking about various business metrics and client relations. Should return "not enough data to answer" or similar.

4. Prompt structure

   It looks like it's susceptible to prompt injection attack. System prompt should be at the top.

5. Debug logging

   Logging full results with business / confidential data especially without proper role setup. GDPR issue arises also.

---

## Section B: Mitigations

*For each risk identified above, propose one concrete, implementable mitigation. Be specific.*

1. **Mitigation for Risk 1:**
   Check roles on every request at orchestrator level or before any query.

2. **Mitigation for Risk 2:**
   Check role specs with SSO provider to identify whether permissions match. Limit endpoint access.

3. **Mitigation for Risk 3:**
   Instruct LLM to return specific message when query does not return results. Do not let the model infer anything.

4. **Mitigation for Risk 4:**
   System prompt should be at the top

5. **Mitigation for Risk 5:**
   Log metadata together with latency, row count. Not result itself. Make sure GDPR compliance is met.

---

## Section C: First Architecture Change

*If you could implement only ONE change to improve this design, what would it be and why?*

**Change:** Fallback behavior.

**Rationale:** Possible bad repercussions + it does seem that this might've been an "innocent" titbit which could be easily missed by candidates. Other than that - RBAC

---

## Section D: Clarifying Questions

*What two questions would you ask stakeholders before implementing or further reviewing this design?*

1. Why session management seems like an afterthought?

2. *[Your second question]*

---

## Section E: Success Metrics (Optional)

*How would you measure whether the chatbot is working correctly and safely? List 1-3 metrics.*

- Roles attempting to access data outside their scope
- Hallucinations
- *[Metric 3]*

---

*End of response.*
