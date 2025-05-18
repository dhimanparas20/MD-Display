# **Detailed Database Schema: Project Models**

## **1. Accounts App**

### **User Model** (extends AbstractUser)
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| username | CharField | | | ✅ | | ❌ | ❌ | | |
| email | EmailField | | | ✅ | | ❌ | ❌ | | |
| password | CharField | | | | | ❌ | ❌ | | |
| user_type | CharField | | | | 'customer' | ❌ | ❌ | superuser, restaurant-owner, customer | |
| gender | CharField | | | | 'not-specify' | ❌ | ❌ | male, female, other, not-specify | |
| first_name | CharField | | | | '' | ✅ | ✅ | | |
| last_name | CharField | | | | '' | ✅ | ✅ | | |
| is_staff | BooleanField | | | | False | ❌ | ❌ | | |
| is_active | BooleanField | | | | True | ❌ | ❌ | | |
| date_joined | DateTimeField | | | | now | ❌ | ❌ | | |

## **2. Chat App**

### **PrivateChat Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| chat_id | CharField | | | ✅ | | ❌ | ❌ | | |
| user1 | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| user2 | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| date_of_creation | DateTimeField | | | | Auto | ❌ | ❌ | | |

### **PrivateChatMessage Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| chat | ForeignKey | | ✅ | | | ❌ | ❌ | | PrivateChat |
| sender | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| user_type | CharField | | | | | ❌ | ❌ | | |
| text | TextField | | | | | ❌ | ❌ | | |
| timestamp | DateTimeField | | | | Auto | ❌ | ❌ | | |

### **GroupChatMessage Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| var_id | CharField | | | | | ❌ | ❌ | | |
| sender | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| user_type | CharField | | | | | ❌ | ❌ | | |
| text | TextField | | | | | ❌ | ❌ | | |
| timestamp | DateTimeField | | | | Auto | ❌ | ❌ | | |

## **3. Customer App**

### **Customer Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| user | OneToOneField | | ✅ | ✅ | | ❌ | ❌ | | User |
| name | CharField | | | | "" | ❌ | ❌ | | |
| one_line_desc | TextField | | | | "" | ❌ | ✅ | | |
| interests | JSONField | | | | list | ❌ | ✅ | 'Beer', 'Wine', etc. | |
| profile | ImageField | | | | | ✅ | ✅ | | |
| current_venue | ForeignKey | | ✅ | | | ✅ | ✅ | | Venue |
| doj | DateTimeField | | | | Auto | ❌ | ❌ | | |
| address | CharField | | | | "" | ❌ | ✅ | | |
| is_verified | BooleanField | | | | False | ❌ | ❌ | | |
| otp_validated | BooleanField | | | | False | ❌ | ❌ | | |
| current_table | PositiveIntegerField | | | | 0 | ❌ | ✅ | | |
| location | CharField | | | | "" | ❌ | ✅ | | |

### **VenuesVisited Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| customer | ForeignKey | | ✅ | | | ❌ | ❌ | | Customer |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| first_visited | DateTimeField | | | | Auto | ❌ | ❌ | | |
| last_visited | DateTimeField | | | | Auto | ❌ | ❌ | | |
| total_visits | PositiveIntegerField | | | | 1 | ❌ | ❌ | | |

### **CustomerToken Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| customer | OneToOneField | | ✅ | ✅ | | ❌ | ❌ | | Customer |
| token_valid_hours | PositiveIntegerField | | | | 24 | ❌ | ❌ | | |
| token_created_on | DateTimeField | | | | Auto | ✅ | ❌ | | |
| token_expiry_on | DateTimeField | | | | | ✅ | ✅ | | |

### **CustomerOTP Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| user | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| email | EmailField | | | | "" | ❌ | ❌ | | |
| venue | ForeignKey | | ✅ | | | ✅ | ✅ | | Venue |
| otp | CharField | | | | | ❌ | ❌ | | |
| otp_validated | BooleanField | | | | False | ❌ | ❌ | | |
| otp_expiry_on | DateTimeField | | | | | ❌ | ❌ | | |

### **CustomerActivityLog Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| customer | ForeignKey | | ✅ | | | ❌ | ❌ | | Customer |
| log_type | CharField | | | | | ❌ | ❌ | venue_signup, venue_login, etc. | |
| description | TextField | | | | | ❌ | ✅ | | |
| timestamp | DateTimeField | | | | Auto | ❌ | ❌ | | |
| related_object_id | CharField | | | | | ✅ | ✅ | | |
| related_object_type | CharField | | | | | ✅ | ✅ | | |

## **4. Platform App**

### **PlatformSettings Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| platform_name | CharField | | | | "" | ❌ | ✅ | | |
| platform_url | URLField | | | | "" | ❌ | ✅ | | |
| support_email | EmailField | | | | "" | ❌ | ✅ | | |
| default_timezone | CharField | | | | 'utc' | ❌ | ❌ | utc, est, cst, mst, pst | |
| date_format | CharField | | | | 'mm,dd,yyyy' | ❌ | ❌ | mm,dd,yyyy, dd,mm,yyyy, yyyy,mm,dd | |
| open_registration | BooleanField | | | | True | ❌ | ❌ | | |
| email_verification | BooleanField | | | | True | ❌ | ❌ | | |
| auto_approve_venues | BooleanField | | | | False | ❌ | ❌ | | |
| default_subscription_plan | CharField | | | | 'basic' | ❌ | ❌ | basic, pro, enterprise | |
| default_trial_period_days | PositiveIntegerField | | | | 10 | ❌ | ❌ | | |
| logo | ImageField | | | | | ✅ | ✅ | | |
| favicon | ImageField | | | | | ✅ | ✅ | | |
| font_family | CharField | | | | "Arial, sans-serif" | ❌ | ❌ | | |
| primary_color | CharField | | | | "#000000" | ❌ | ✅ | | |
| secondary_color | CharField | | | | "#FFFFFF" | ❌ | ✅ | | |
| default_theme | CharField | | | | 'system' | ❌ | ❌ | light, dark, system | |
| allow_users_to_change_theme | BooleanField | | | | True | ❌ | ❌ | | |
| border_radius | CharField | | | | 'medium' | ❌ | ❌ | none, small, medium, large | |
| marketing_emails | BooleanField | | | | True | ❌ | ❌ | | |
| social_notifications | BooleanField | | | | True | ❌ | ❌ | | |
| security_alerts | BooleanField | | | | True | ❌ | ❌ | | |
| platform_updates | BooleanField | | | | True | ❌ | ❌ | | |
| welcome_email_template | TextField | | | | "" | ❌ | ✅ | | |
| email_verification_template | TextField | | | | "" | ❌ | ✅ | | |
| password_reset_template | TextField | | | | "" | ❌ | ✅ | | |
| require_two_factor_auth | BooleanField | | | | False | ❌ | ❌ | | |
| session_timeout_minutes | PositiveIntegerField | | | | 60 | ❌ | ❌ | | |
| password_policy | CharField | | | | 'basic' | ❌ | ❌ | basic, medium, strong | |
| password_expiry_days | PositiveIntegerField | | | | 90 | ❌ | ❌ | | |
| enable_api_access | BooleanField | | | | False | ❌ | ❌ | | |
| api_rate_limit | PositiveIntegerField | | | | 60 | ❌ | ❌ | | |
| default_currency | CharField | | | | 'usd' | ❌ | ❌ | usd, eur, gbp, cad, aud | |
| default_tax_rate | DecimalField(5,2) | | | | 0 | ❌ | ❌ | | |
| tax_inclusive_pricing | BooleanField | | | | False | ❌ | ❌ | | |
| credit_debit_cards | BooleanField | | | | True | ❌ | ❌ | | |
| paypal | BooleanField | | | | False | ❌ | ❌ | | |
| apple_pay | BooleanField | | | | False | ❌ | ❌ | | |
| google_pay | BooleanField | | | | False | ❌ | ❌ | | |
| company_name_invoice | CharField | | | | "" | ❌ | ✅ | | |
| company_address_invoice | TextField | | | | "" | ❌ | ✅ | | |
| vat_tax_id_invoice | CharField | | | | "" | ❌ | ✅ | | |
| invoice_number_prefix | CharField | | | | "" | ❌ | ✅ | | |
| invoice_footer_text | TextField | | | | "" | ❌ | ✅ | | |

## **5. Venue App**

### **Venue Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| name | CharField | | | | | ❌ | ❌ | | |
| description | TextField | | | | "" | ❌ | ✅ | | |
| logo | ImageField | | | | | ✅ | ✅ | | |
| owner | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| venue_type | CharField | | | | 'restaurant' | ❌ | ❌ | restaurant, bar, cafe, pub, etc. | |
| capacity | PositiveIntegerField | | | | 0 | ❌ | ❌ | | |
| is_active | BooleanField | | | | True | ❌ | ❌ | | |
| require_otp | BooleanField | | | | False | ❌ | ❌ | | |
| date_created | DateTimeField | | | | Auto | ❌ | ❌ | | |
| insta_url | URLField | | | | "" | ❌ | ✅ | | |
| fb_url | URLField | | | | "" | ❌ | ✅ | | |
| twitter_url | URLField | | | | "" | ❌ | ✅ | | |
| venue_webiste | URLField | | | | "" | ❌ | ✅ | | |

### **VenueAddress Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | OneToOneField | | ✅ | ✅ | | ❌ | ❌ | | Venue |
| address | CharField | | | | "" | ❌ | ✅ | | |
| city | CharField | | | | "" | ❌ | ✅ | | |
| state | CharField | | | | "" | ❌ | ✅ | | |
| postal_code | PositiveIntegerField | | | | 0 | ❌ | ✅ | | |
| country | CharField | | | | "" | ❌ | ✅ | | |
| contact | CharField | | | | "" | ❌ | ✅ | | |
| business_email | EmailField | | | | "" | ❌ | ✅ | | |
| contact_name | CharField | | | | "" | ❌ | ✅ | | |
| position | CharField | | | | "" | ❌ | ✅ | | |

### **VenueBusinessDetails Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | OneToOneField | | ✅ | ✅ | | ❌ | ❌ | | Venue |
| subscription_plan | CharField | | | | 'starter' | ❌ | ❌ | starter, professional, enterprise | |
| revenue | PositiveIntegerField | | | | 0 | ❌ | ❌ | | |
| total_customers | PositiveIntegerField | | | | 0 | ❌ | ❌ | | |
| total_messages | PositiveIntegerField | | | | 0 | ❌ | ❌ | | |
| total_qr_scanned | PositiveIntegerField | | | | 0 | ❌ | ❌ | | |

### **PaymentDetails Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | OneToOneField | | ✅ | ✅ | | ❌ | ❌ | | Venue |
| card_no | CharField | | | | | ❌ | ❌ | | |
| expiry_month | PositiveSmallIntegerField | | | | 1 | ❌ | ✅ | | |
| expiry_year | PositiveSmallIntegerField | | | | 2030 | ❌ | ✅ | | |
| cvc | CharField | | | | | ❌ | ❌ | | |
| name_on_card | CharField | | | | | ❌ | ❌ | | |
| billing_address | CharField | | | | | ❌ | ❌ | | |
| zipcode | CharField | | | | | ❌ | ❌ | | |
| city | CharField | | | | | ❌ | ❌ | | |
| country | CharField | | | | | ❌ | ❌ | | |
| subscription_start_date | DateTimeField | | | | Auto | ❌ | ❌ | | |
| payment_date | DateTimeField | | | | now | ❌ | ❌ | | |
| payment_amount | DecimalField(10,2) | | | | 0 | ❌ | ❌ | | |
| last_update | DateTimeField | | | | Auto | ❌ | ❌ | | |

### **VenueQRSettings Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | OneToOneField | | ✅ | ✅ | | ❌ | ❌ | | Venue |
| var_id | CharField | | | ✅ | "" | ❌ | ✅ | | |
| var_id_gen_time | DateTimeField | | | | now | ❌ | ❌ | | |
| var_id_expiry_time | DateTimeField | | | | | ✅ | ✅ | | |
| qr_gen_frequency_text | CharField | | | | 'weekly' | ❌ | ❌ | hourly, daily, weekly, monthly | |
| qr_gen_frequency_hours | PositiveIntegerField | | | | 168 | ❌ | ❌ | 1, 24, 168, 730 | |
| customer_token_refresh_frequency | PositiveIntegerField | | | | 168 | ❌ | ❌ | | |

### **VenueOffer Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | UUIDField | ✅ | | ✅ | uuid4 | ❌ | ❌ | | |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| token | CharField | | | ✅ | "" | ❌ | ✅ | | |
| description | TextField | | | | "" | ❌ | ✅ | | |
| discount_type | CharField | | | | 'percentage' | ❌ | ❌ | percentage, fixed | |
| discount_value | DecimalField(10,2) | | | | 1 | ❌ | ❌ | | |
| min_purchase_amount | DecimalField(10,2) | | | | 0 | ❌ | ❌ | | |
| applicable_to | CharField | | | | 'all' | ❌ | ❌ | all, profesional, enterprise, starters | |
| created_at | DateTimeField | | | | Auto | ❌ | ❌ | | |
| status | CharField | | | | 'scheduled' | ❌ | ❌ | active, expired, scheduled | |
| valid_from | DateTimeField | | | | now | ✅ | ✅ | | |
| valid_to | DateTimeField | | | | | ✅ | ✅ | | |
| time_based | BooleanField | | | | False | ❌ | ❌ | | |
| time_duration_hours | PositiveIntegerField | | | | 1 | ✅ | ✅ | | |
| expiry_time | DateTimeField | | | | | ✅ | ✅ | | |
| limited_users | BooleanField | | | | False | ❌ | ❌ | | |
| limited_users_count | PositiveIntegerField | | | | 100 | ✅ | ✅ | | |
| uses_per_user | PositiveIntegerField | | | | 1 | ✅ | ✅ | | |
| new_customers_only | BooleanField | | | | False | ❌ | ❌ | | |
| total_redumptions | PositiveSmallIntegerField | | | | 0 | ❌ | ❌ | | |

### **VenueOfferRedemption Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| customer | ForeignKey | | ✅ | | | ❌ | ❌ | | Customer |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| offer | ForeignKey | | ✅ | | | ❌ | ❌ | | VenueOffer |
| token | CharField | | | | | ❌ | ❌ | | |
| offer_description | TextField | | | | | ❌ | ❌ | | |
| redeemed_at | DateTimeField | | | | Auto | ❌ | ❌ | | |

### **VenueSignup Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| customer | ForeignKey | | ✅ | | | ❌ | ❌ | | Customer |
| join_time | DateTimeField | | | | Auto | ❌ | ❌ | | |
| leave_time | DateTimeField | | | | | ✅ | ✅ | | |
| is_active | BooleanField | | | | True | ❌ | ❌ | | |

### **TableReservation Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| name | CharField | | | | "" | ❌ | ❌ | | |
| user | ForeignKey | | ✅ | | | ❌ | ❌ | | User |
| reservation_time | DateTimeField | | | | Auto | ❌ | ❌ | | |
| party_size | PositiveIntegerField | | | | 1 | ❌ | ❌ | | |
| table | CharField | | | | | ❌ | ❌ | | |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| status | CharField | | | | 'pending' | ❌ | ❌ | confirmed, seated, pending, rejected | |

### **VenueActivityLog Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| venue | ForeignKey | | ✅ | | | ❌ | ❌ | | Venue |
| log_type | CharField | | | | | ❌ | ❌ | signup, login, logout, etc. | |
| description | TextField | | | | | ❌ | ✅ | | |
| timestamp | DateTimeField | | | | Auto | ❌ | ❌ | | |
| related_object_id | CharField | | | | | ✅ | ✅ | | |
| related_object_type | CharField | | | | | ✅ | ✅ | | |

### **IceBreaker Model**
| Field | Type | PK | FK | Unique | Default | Null | Blank | Choices | Related Model |
|-------|------|----|----|--------|---------|------|-------|---------|--------------|
| id | AutoField | ✅ | | ✅ | Auto | ❌ | ❌ | | |
| type | CharField | | | | | ❌ | ❌ | question, joke, compliment, pickup_line | |
| content | TextField | | | | | ❌ | ❌ | | |
| level | PositiveIntegerField | | | | | ❌ | ❌ | 1, 2, 3, 4, 5 | |
| gender | CharField | | | | | ✅ | ✅ | male, female, Any | |
| created_at | DateTimeField | | | | Auto | ❌ | ❌ | | |
| is_active | BooleanField | | | | True | ❌ | ❌ | | |
