# UEEx World Cup Prediction Bot

Telegram bot for UEEx World Cup score prediction activity.

Users can select World Cup matches, predict exact scores, vote with UE, and track their orders through the bot.

## Main Features

* Telegram private bot flow
* Language selection: English / Chinese
* Match list by date
* Exact score prediction
* UE voting amount input
* UID binding
* Automatic payment matching by UID + amount
* Manual order confirmation
* Order cancellation
* Public match cards in Telegram topic
* Order confirmed notifications
* Match settlement and winner calculation
* Carryover pool to World Cup Final when no exact-score winner
* Different minimum vote amounts by match stage

## Minimum Vote Amount

The bot supports different minimum vote amounts based on match stage:

| Match Stage           | Minimum Vote |
| --------------------- | -----------: |
| Group Stage / Default |     1,000 UE |
| Round of 32           |     2,000 UE |
| Round of 16           |     3,000 UE |
| Quarter Final         |     4,000 UE |
| Semi Final            |     5,000 UE |
| Final                 |    10,000 UE |

Recommended stage names when creating matches:

```text
Round-Of-32
Round-Of-16
Quarter-Final
Semi-Final
Final
```

Example:

```text
/worldcup_ZAF_CAN_0:0_3:3_Others_2026.06.28_23:00_UTC+4_Round-Of-32
```

## Project Files

```text
index.js
package.json
package-lock.json
README.md
```

## Install

```bash
npm install
```

## Run Locally

```bash
node index.js
```

## Render Deploy Settings

Build Command:

```bash
npm install
```

Start Command:

```bash
node index.js
```

## Required Environment Variables

Do not upload real secrets to GitHub.

```env
BOT_TOKEN=
BOT_USERNAME=
WEBHOOK_URL=

SUPABASE_URL=
SUPABASE_SERVICE_ROLE_KEY=

ADMIN_GROUP_CHAT_ID=
ADMIN_USER_IDS=

PUBLIC_GROUP_CHAT_ID=
PUBLIC_WORLD_CUP_TOPIC_ID=
PUBLIC_GROUP_USERNAME=
PUBLIC_WORLD_CUP_TOPIC_URL=

RECEIVER_UID=
TRANSFER_ADDRESS=
DEFAULT_CURRENCY=UE
```

## Payment / Auto Confirmation Variables

```env
AUTO_CONFIRM_ENABLED=true
PAYMENT_MATCH_MODE=remark_or_uid_amount
UID_AMOUNT_MATCH_ALLOW_REMARK=true

UEEX_API_BASE_URL=
UEEX_API_KEY=
UEEX_API_SECRET=
UEEX_API_TOKEN=
UEEX_API_DEPOSIT_LIST_PATH=
UEEX_PAYMENT_ITEM_ID=
UEEX_PAYMENT_TYPE=
UEEX_RECEIVER_UID=
UEEX_UID_MATCH_MODE=
UEEX_INTERNAL_EXCHANGE_TYPE=
UEEX_SUCCESS_STATUS=
```

## Minimum Vote Environment Variables

These values already have defaults in the code, but they can also be configured in Render.

```env
MIN_BET_AMOUNT=1000
MIN_BET_AMOUNT_ROUND_32=2000
MIN_BET_AMOUNT_ROUND_16=3000
MIN_BET_AMOUNT_QUARTER_FINAL=4000
MIN_BET_AMOUNT_SEMI_FINAL=5000
MIN_BET_AMOUNT_FINAL=10000
```

## Webhook

The bot uses Telegram webhook mode.

Set `WEBHOOK_URL` to your Render service URL:

```env
WEBHOOK_URL=https://your-render-service.onrender.com
```

The bot will automatically register:

```text
https://your-render-service.onrender.com/telegram
```

## Health Check

After deployment, open:

```text
https://your-render-service.onrender.com/
```

If the service is running, it should show:

```text
UEEx World Cup Bot is running.
```

You can also test the Telegram bot with:

```text
/ping
```

Expected reply:

```text
pong
```

## Important Notes

* Only one Render service should run the same Telegram Bot Token at the same time.
* If multiple services use the same Bot Token, the latest deployed service may overwrite the Telegram webhook.
* Keep the old Render service suspended or deleted before using the new one.
* Supabase does not need to be migrated if you continue using the same database.
* Never commit `.env`, API keys, bot token, or Supabase service role key to GitHub.

## Admin Match Creation Format

```text
/worldcup_Team1_Team2_MinScore_MaxScore_Others_Date_Time_Timezone_Stage
```

Example:

```text
/worldcup_ARG_AUT_0:0_3:3_Others_2026.06.22_21:00_UTC+4_Group-J
```

## Common Admin Commands

Confirm order:

```text
/confirm_ORDERID_AMOUNT
```

Void pending order:

```text
/void_ORDERID
```

Publish result:

```text
/result_MATCHCODE_SCORE
```

Example:

```text
/result_WCZ15P_2:0
```

Check matches that need result:

```text
/need_result
```

Close no-bet match:

```text
/no_bet_MATCHCODE
```

## Notes for Operators

* Users no longer need to fill the Order ID as a mandatory transfer remark.
* The system can match payment by UID + exact amount.
* If users fill an Order ID as remark, it can still help with manual review.
* Voting closes 15 minutes before match kick-off.
* Settlement score is based on official 90-minute result, including stoppage time, excluding extra time and penalties.
* If no one hits the exact score, the net pool rolls over to the World Cup Final pool.
