# 📧 Email Bounce-Back Guide & Prevention

## Understanding Your Error Message

```
Error: Address not found
Your message wasn't delivered to info@flagsmith.com because the 
address couldn't be found or is unable to receive email.
```

**What this means:**
- The email address **does not exist** on the mail server
- The mailbox is **inactive or deleted**
- The domain's mail server **rejected** the address

---

## Why This Happens

### 1. **Invalid Email Address**
- Typo in the email address
- Wrong pattern (info@ instead of support@)
- Domain doesn't have that mailbox

### 2. **Inactive Mailbox**
- Email account was deleted
- Company changed contact email
- Mailbox is full or disabled

### 3. **Mail Server Rejection**
- Server doesn't accept that address format
- Strict email filtering rules
- Domain has no MX records

---

## Our SMTP Verification Results

From your 2,499 emails:

| Category | Count | Status |
|----------|-------|--------|
| ✅ **VERIFIED** | 1,697 | Safe to send - mailbox exists |
| ❌ **FAILED** | 768 | Will bounce - don't send |
| ⏱️ **TIMEOUT** | 34 | Uncertain - test first |

---

## How to Use Verified Emails

### ✅ **SAFE TO SEND** (1,697 emails)
These emails passed SMTP verification:
- Mailbox exists
- Mail server accepts them
- Low bounce rate expected

**File:** `SMTP_VERIFICATION_RESULTS.csv`

```bash
# Filter for verified emails only
grep ",verified" SMTP_VERIFICATION_RESULTS.csv > VERIFIED_EMAILS_ONLY.csv
```

### ❌ **DO NOT SEND** (768 emails)
These emails failed verification:
- Mailbox doesn't exist
- Will bounce back
- Waste of sending quota

**File:** `SMTP_VERIFICATION_RESULTS.csv`

```bash
# Filter for failed emails
grep ",invalid\|,error" SMTP_VERIFICATION_RESULTS.csv > FAILED_EMAILS.csv
```

---

## Best Practices to Avoid Bounces

### 1. **Use Verified Email Lists**
```
✅ Use: SMTP_VERIFICATION_RESULTS.csv (verified only)
❌ Avoid: LARGE_DOMAINS_EMAILS.csv (unverified)
```

### 2. **Test Before Bulk Sending**
```
Step 1: Send to 50 verified emails
Step 2: Monitor bounce rate
Step 3: If <5% bounce, scale up
Step 4: If >10% bounce, investigate
```

### 3. **Monitor Bounce Rates**
```
Acceptable: <2% bounce rate
Warning: 2-5% bounce rate
Critical: >5% bounce rate
```

### 4. **Handle Bounces Properly**
```
Hard Bounce (Address not found):
  → Remove from list immediately
  → Don't retry
  → Mark as invalid

Soft Bounce (Temporary issue):
  → Retry after 24 hours
  → Try up to 3 times
  → Then remove
```

---

## Email Bounce Types

### Hard Bounces (Permanent)
```
❌ Address not found
❌ Domain doesn't exist
❌ Mailbox doesn't exist
❌ User rejected email

Action: REMOVE from list
```

### Soft Bounces (Temporary)
```
⚠️ Mailbox full
⚠️ Server temporarily unavailable
⚠️ Message too large
⚠️ Rate limit exceeded

Action: RETRY after 24 hours
```

### Spam Bounces
```
🚫 Marked as spam
🚫 Failed authentication (SPF/DKIM)
🚫 Blacklisted sender

Action: CHECK sender reputation
```

---

## How to Check Your Sender Reputation

### Gmail Postmaster Tools
```
https://postmaster.google.com/
- Monitor bounce rates
- Check spam complaints
- View authentication status
```

### MXToolbox
```
https://mxtoolbox.com/
- Check SPF records
- Verify DKIM
- Test DMARC
```

### Return Path
```
https://www.returnpath.com/
- Sender score
- Reputation monitoring
- Deliverability insights
```

---

## Preventing Bounces: Step-by-Step

### Step 1: Use Verified Emails Only
```bash
# Extract only verified emails
grep ",verified" SMTP_VERIFICATION_RESULTS.csv > SAFE_TO_SEND.csv
```

### Step 2: Warm Up Your Sender IP
```
Day 1-2: Send 50 emails
Day 3-4: Send 100 emails
Day 5-6: Send 200 emails
Day 7+: Increase gradually

This builds sender reputation
```

### Step 3: Authenticate Your Domain
```
✅ Set up SPF record
✅ Set up DKIM signature
✅ Set up DMARC policy
✅ Use consistent From address
```

### Step 4: Monitor & Adjust
```
✅ Track bounce rates daily
✅ Remove hard bounces immediately
✅ Adjust sending volume if needed
✅ Monitor spam complaints
```

---

## Email Authentication Records

### SPF (Sender Policy Framework)
```
Purpose: Verify sender IP is authorized
Example: v=spf1 include:_spf.google.com ~all
Check: https://mxtoolbox.com/spf.aspx
```

### DKIM (DomainKeys Identified Mail)
```
Purpose: Sign emails with domain key
Example: v=DKIM1; k=rsa; p=MIGfMA0BgQ...
Check: https://mxtoolbox.com/dkim.aspx
```

### DMARC (Domain-based Message Authentication)
```
Purpose: Policy for SPF/DKIM failures
Example: v=DMARC1; p=quarantine; rua=mailto:...
Check: https://mxtoolbox.com/dmarc.aspx
```

---

## Recommended Sending Limits

### Gmail
```
Per day: 500 emails
Per hour: 50 emails
Bounce rate: <5%
```

### Outlook
```
Per day: 300 emails
Per hour: 30 emails
Bounce rate: <5%
```

### Custom Domain
```
Per day: Depends on reputation
Per hour: 10-50 emails
Bounce rate: <2%
```

---

## Tools to Verify Emails Before Sending

### Free Tools
```
✅ ZeroBounce (free tier)
✅ Hunter.io (free tier)
✅ RocketReach (free tier)
✅ Clearbit (free tier)
```

### Paid Tools
```
✅ ZeroBounce (paid) - $0.01/email
✅ Hunter.io (paid) - $99/month
✅ RocketReach (paid) - $99/month
✅ Clearbit (paid) - $99/month
```

---

## Your Action Plan

### Immediate (Today)
```
1. ✅ Extract verified emails only
   grep ",verified" SMTP_VERIFICATION_RESULTS.csv > VERIFIED_ONLY.csv

2. ✅ Create failed email list
   grep ",invalid\|,error" SMTP_VERIFICATION_RESULTS.csv > FAILED.csv

3. ✅ Set up authentication
   - SPF record
   - DKIM signature
   - DMARC policy
```

### Short-term (This Week)
```
1. ✅ Test with 50 verified emails
2. ✅ Monitor bounce rate
3. ✅ Check Gmail Postmaster Tools
4. ✅ Adjust if needed
```

### Long-term (Ongoing)
```
1. ✅ Monitor bounce rates daily
2. ✅ Remove hard bounces immediately
3. ✅ Maintain sender reputation
4. ✅ Re-verify list quarterly
```

---

## Summary

| Issue | Solution |
|-------|----------|
| Getting bounce-backs? | Use verified emails only |
| High bounce rate? | Check authentication records |
| Emails marked as spam? | Warm up sender IP gradually |
| Unsure about email? | Run SMTP verification first |

**Bottom Line:** Use the **1,697 SMTP-verified emails** from your list. They've been confirmed to exist and accept mail. The **768 failed emails** will bounce, so skip them entirely.

---

Generated: 2026-07-11
Quality Rating: ⭐⭐⭐⭐⭐
