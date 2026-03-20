# FoxReach API Reference

## Base Configuration

| Setting | Value |
|---------|-------|
| Base URL | `https://api.foxreach.io/api/v1` |
| Auth Header | `X-API-Key` |
| Key Prefix | `otr_` |
| Rate Limit | 100 requests/minute |
| Response Format | `{ data: T }` or `{ data: T[], meta: { page, pageSize, total, totalPages } }` |

---

## Data Types

### Lead

```python
@dataclass(frozen=True)
class Lead:
    id: str                                    # e.g. "cld_abc123def456"
    email: str                                 # required, unique per workspace
    first_name: Optional[str]
    last_name: Optional[str]
    company: Optional[str]
    title: Optional[str]                       # job title
    phone: Optional[str]
    linkedin_url: Optional[str]
    website: Optional[str]
    custom_fields: Optional[Dict[str, Any]]    # arbitrary key-value pairs
    notes: Optional[str]
    tags: List[str]                            # tag names
    status: str                                # "active" | "bounced" | "unsubscribed" | "replied"
    source: Optional[str]                      # "api" | "csv_import" | "manual"
    created_at: Optional[str]                  # ISO 8601
    updated_at: Optional[str]
```

### LeadCreate

```python
@dataclass
class LeadCreate:
    email: str                                 # required
    first_name: Optional[str] = None
    last_name: Optional[str] = None
    company: Optional[str] = None
    title: Optional[str] = None
    phone: Optional[str] = None
    linkedin_url: Optional[str] = None
    website: Optional[str] = None
    custom_fields: Optional[Dict[str, Any]] = None
    tags: Optional[List[str]] = None
    notes: Optional[str] = None
```

### LeadUpdate

Same fields as LeadCreate, all optional. Only non-None fields are sent.

---

### Campaign

```python
@dataclass(frozen=True)
class Campaign:
    id: str                       # e.g. "cmp_xyz789ghi012"
    name: str
    status: str                   # "draft" | "active" | "paused" | "completed"
    timezone: str                 # IANA timezone e.g. "America/New_York"
    sending_days: List[int]       # 1=Mon, 2=Tue, ..., 7=Sun
    sending_start_hour: int       # 0-23
    sending_end_hour: int         # 0-23
    sending_start_minute: int     # 0-59
    sending_end_minute: int       # 0-59
    daily_limit: int              # max emails per day
    sent_today: int
    queued_today: int             # emails queued but not yet sent today
    context: Optional[str]        # campaign context/notes
    total_leads: int
    total_sent: int
    total_delivered: int
    total_bounced: int
    total_replied: int
    total_opened: int
    created_at: Optional[str]
    updated_at: Optional[str]
    started_at: Optional[str]
    completed_at: Optional[str]
```

### CampaignCreate

```python
@dataclass
class CampaignCreate:
    name: str                                       # required
    timezone: str = "UTC"
    sending_days: Optional[List[int]] = None        # default: [1,2,3,4,5]
    sending_start_hour: Optional[int] = None        # default: 9
    sending_end_hour: Optional[int] = None          # default: 17
    daily_limit: Optional[int] = None               # default: 50
```

### CampaignUpdate

Same fields as CampaignCreate but all optional including `name`.

---

### Sequence

```python
@dataclass(frozen=True)
class Sequence:
    id: str
    campaign_id: str
    name: Optional[str]           # step name/label
    step_number: int              # order in the sequence chain
    subject: Optional[str]        # email subject (supports {{variables}})
    body: str                     # email body (supports {{variables}} and spintax)
    delay_days: int               # days to wait after previous step
    delay_hours: int              # additional hours to wait after delay_days
    send_only_if_no_reply: bool   # skip step if lead already replied
    send_as_html: bool            # send as HTML (vs plain text)
    variants: Optional[List]      # A/B test variants
    has_variants: bool            # whether step has A/B variants
    created_at: Optional[str]
    updated_at: Optional[str]
```

### SequenceCreate

```python
@dataclass
class SequenceCreate:
    body: str                                  # required
    subject: Optional[str] = None
    name: Optional[str] = None
    delay_days: int = 1
    delay_hours: int = 0
    step_number: Optional[int] = None          # auto-assigned if omitted
    send_only_if_no_reply: bool = True
    send_as_html: bool = False
```

### SequenceUpdate

```python
@dataclass
class SequenceUpdate:
    subject: Optional[str] = None
    body: Optional[str] = None
    name: Optional[str] = None
    delay_days: Optional[int] = None
    delay_hours: Optional[int] = None
    step_number: Optional[int] = None
    send_only_if_no_reply: Optional[bool] = None
    send_as_html: Optional[bool] = None
```

---

### Template

```python
@dataclass(frozen=True)
class Template:
    id: str
    name: str
    subject: Optional[str]
    body: str
    category: str                 # "email" | "snippet" | etc.
    send_as_html: bool
    created_at: Optional[str]
    updated_at: Optional[str]
```

### TemplateCreate

```python
@dataclass
class TemplateCreate:
    name: str                     # required
    body: str                     # required
    subject: Optional[str] = None
    category: str = "email"       # "email" | "snippet" | etc.
    send_as_html: bool = False
```

---

### EmailAccount

```python
@dataclass(frozen=True)
class EmailAccount:
    id: str
    email: str
    display_name: Optional[str]
    provider: Optional[str]
    daily_limit: int
    sent_today: int
    is_active: bool
    warmup_enabled: bool
    health_score: Optional[float]     # 0-100
    bounce_rate: Optional[float]      # 0-100 percentage
    reply_rate: Optional[float]       # 0-100 percentage
    connection_status: Optional[str]
    created_at: Optional[str]
    updated_at: Optional[str]
```

---

### Thread (Inbox Reply)

```python
@dataclass(frozen=True)
class Thread:
    id: str                           # e.g. "rpl_abc123"
    from_email: Optional[str]
    to_email: Optional[str]
    subject: Optional[str]
    body: Optional[str]               # HTML body
    body_plain: Optional[str]         # plain text body
    category: Optional[str]           # "interested" | "not_interested" | "out_of_office" | etc.
    is_read: bool
    is_starred: bool
    campaign_id: Optional[str]
    lead_id: Optional[str]
    account_id: Optional[str]
    received_at: Optional[str]
```

### ThreadUpdate

```python
@dataclass
class ThreadUpdate:
    category: Optional[str] = None
    is_read: Optional[bool] = None
    is_starred: Optional[bool] = None
```

---

### OverviewStats

```python
@dataclass(frozen=True)
class OverviewStats:
    total_accounts: int
    active_accounts: int
    total_campaigns: int
    active_campaigns: int
    total_leads: int
    total_sent: int
    total_replies: int
    reply_rate: float                 # percentage
    account_health_avg: float         # 0-100
```

### CampaignAnalytics

```python
@dataclass(frozen=True)
class CampaignAnalytics:
    sent: int
    delivered: int
    bounced: int
    replied: int
    opened: int
    reply_rate: float
    bounce_rate: float
    daily_stats: List[DailyStat]      # per-day breakdown
```

### DailyStat

```python
@dataclass(frozen=True)
class DailyStat:
    date: str                         # "2025-01-15"
    sent: int
    delivered: int
    bounced: int
    replied: int
    opened: int
```

---

### Webhook

```python
@dataclass(frozen=True)
class Webhook:
    id: str
    url: str
    is_active: bool
    events: List[str]                     # list of subscribed event types
    last_delivered_at: Optional[str]      # ISO 8601
    consecutive_failures: int
    created_at: Optional[str]
    updated_at: Optional[str]
```

### WebhookCreate

```python
@dataclass
class WebhookCreate:
    url: str                              # required — endpoint URL
    events: List[str]                     # required — event types to subscribe to
    is_active: bool = True
```

### WebhookUpdate

```python
@dataclass
class WebhookUpdate:
    url: Optional[str] = None
    events: Optional[List[str]] = None
    is_active: Optional[bool] = None
```

---

### WebhookEvent

```python
@dataclass(frozen=True)
class WebhookEvent:
    id: str                           # e.g. "evt_sample001"
    type: str                         # see Webhook Event Types below
    payload: Dict[str, Any]
    timestamp: Optional[str]
```

### Webhook Event Types

| Event | Description |
|-------|-------------|
| `reply.received` | A reply is received and matched to a lead/campaign |
| `reply.categorized` | A reply is categorized by AI or manually |
| `email.bounced` | A sent email bounces back |
| `email.sent` | An email is successfully sent from a campaign |
| `email.failed` | An email fails to send |
| `email.opened` | A recipient opens a tracked email |
| `campaign.started` | A campaign transitions to active status |
| `campaign.paused` | A campaign is paused |
| `campaign.completed` | All leads in a campaign have been processed |
| `lead.created` | A new lead is created |
| `lead.updated` | Lead information is updated |

---

## API Endpoints

### Leads — `/leads`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/leads` | List leads. Query: `page`, `pageSize`, `search`, `status`, `tags` |
| GET | `/leads/{id}` | Get lead by ID |
| POST | `/leads` | Create lead. Deduplicates by email |
| PATCH | `/leads/{id}` | Update lead |
| DELETE | `/leads/{id}` | Soft-delete lead |

#### Lead Activity

| Method | Path | Description |
|--------|------|-------------|
| GET | `/leads/{id}/activity` | Get lead activity. Returns `{ activities: [...], campaigns: [...] }` |

### Campaigns — `/campaigns`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/campaigns` | List campaigns. Query: `page`, `pageSize`, `status` |
| GET | `/campaigns/{id}` | Get campaign with stats |
| POST | `/campaigns` | Create campaign (starts as draft) |
| PATCH | `/campaigns/{id}` | Update campaign |
| DELETE | `/campaigns/{id}` | Delete (must be draft) |
| POST | `/campaigns/{id}/start` | Start campaign |
| POST | `/campaigns/{id}/pause` | Pause campaign |
| POST | `/campaigns/{id}/resume` | Resume paused campaign |
| GET | `/campaigns/{id}/sequences` | List sequence steps |
| POST | `/campaigns/{id}/sequences` | Add sequence step |
| PATCH | `/campaigns/{id}/sequences/{seq_id}` | Update step |
| DELETE | `/campaigns/{id}/sequences/{seq_id}` | Delete step |
| POST | `/campaigns/{id}/leads` | Add leads. Body: `{ leadIds: [...] }` |
| DELETE | `/campaigns/{id}/leads/{lead_id}` | Remove lead from campaign |
| POST | `/campaigns/{id}/accounts` | Assign accounts. Body: `{ accountIds: [...] }` |
| DELETE | `/campaigns/{id}/accounts/{account_id}` | Remove account from campaign |

### Templates — `/templates`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/templates` | List templates |
| GET | `/templates/{id}` | Get template |
| POST | `/templates` | Create template |
| PATCH | `/templates/{id}` | Update template |
| DELETE | `/templates/{id}` | Delete template |

### Email Accounts — `/email-accounts`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/email-accounts` | List accounts |
| GET | `/email-accounts/{id}` | Get account with health |
| DELETE | `/email-accounts/{id}` | Delete account |

### Inbox — `/inbox`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/inbox/threads` | List threads. Query: `page`, `pageSize`, `category`, `isRead`, `isStarred`, `accountId`, `campaignId`, `search` |
| GET | `/inbox/{id}` | Get thread |
| PATCH | `/inbox/{id}` | Update thread (category, read, starred) |
| GET | `/inbox/threads/{id}/conversation` | Full conversation thread with all messages |
| POST | `/inbox/threads/{id}/reply` | Send reply. Body: `{ body: str, subject?: str }` |
| GET | `/inbox/stats` | Inbox statistics (counts by category, unread totals) |

### Analytics — `/analytics`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/analytics/overview` | Dashboard KPIs |
| GET | `/analytics/campaigns/{id}` | Campaign metrics + daily stats |

### Webhooks — `/webhooks`

| Method | Path | Description |
|--------|------|-------------|
| GET | `/webhooks/events` | List available event types |
| GET | `/webhooks` | List webhooks. Query: `page`, `pageSize` |
| POST | `/webhooks` | Create webhook. Returns secret on creation |
| PATCH | `/webhooks/{id}` | Update webhook |
| DELETE | `/webhooks/{id}` | Delete webhook |
| POST | `/webhooks/inbound` | Incoming webhook receiver |
