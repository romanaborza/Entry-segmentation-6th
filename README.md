# Entry Segmentation — Implementation Notes

## Optional vs Mandatory Questions

1. If a screen contains the label **(nepovinné)**, the question is **optional** — the user does not need to fill it in to proceed to the next screen.

2. The following questions are **mandatory** and must be answered before the user can proceed:
   - Proč zakládáte e-shop?
   - Co nejlépe popisuje vaši situaci?
   - Jste Shoptet Partner?

   If the user clicks **"Pokračovat"** without selecting an answer, display an error message. Reference design: [Error state – Figma](https://www.figma.com/design/EkHwH4T3uKphPu9p0Mu3Uf/Entry-Segmentation?node-id=352-4745&t=5t13ihr7LT2ZVFg5-4)

3. On the **Jste Shoptet Partner?** screen:
   - The question is **mandatory** — the user cannot proceed without selecting an option.
   - **Ne, nejsem Shoptet Partner** → flow continues: Mobil → Typ subjektu + IČO
   - **Ano, jsem Shoptet Partner** → the **Partnerský kód** field becomes **mandatory**. The user cannot proceed until the code is filled in. Once filled, the flow continues: Mobil → Typ subjektu + IČO

## Screen Flow

To implement the correct navigation between screens, follow the flow diagram:
[Flow diagram – Figma](https://www.figma.com/design/EkHwH4T3uKphPu9p0Mu3Uf/Entry-Segmentation?node-id=345-4036&t=5t13ihr7LT2ZVFg5-4)

### Navigation paths from "Proč zakládáte e-shop?"

| Answer | Next screens |
|---|---|
| Zatím nemám e-shop | → Co nejlépe popisuje vaši situaci? → Mobil → Typ subjektu |
| Chci svůj e-shop převést na Shoptet | → URL (migrace) → Mobil (migrace) → Typ subjektu |
| Už e-shop na Shoptetu mám a chci pořídit další | → URL (existující e-shop) → Mobil → Typ subjektu |
| Jiné (regardless of sub-question selection) | → Mobil → Typ subjektu |

**Jiné — sub-question behaviour:** When the user selects **Jiné**, the optional sub-question *"Jaký je váš důvod? (nepovinné)"* appears on the same screen with three options:

| Sub-option | Auto-advance | Next screens |
|---|---|---|
| Jsem Shoptet partner a vytvářím e-shop pro klienta | ✅ yes | → Jaký je váš partnerský kód? → Mobil |
| Vytvářím e-shop pro klienta | ✅ yes | → Mobil → Typ subjektu + IČO |
| Jiné | ❌ no — user stays to optionally type their reason, then presses "Pokračovat" | → Mobil → Typ subjektu + IČO |

Selecting **Jiné** reveals a text area *"Popište váš důvod"* inside the card. Filling it in is optional. The user must press "Pokračovat" to proceed.

If the user selects nothing in the sub-question and presses "Pokračovat", the flow continues to **Mobil → Typ subjektu + IČO**.

## Input Behaviour

- **Radio buttons** — the user can select **only one option**. Selecting a new option automatically deselects the previous one.

- **Auto-advance** — on screens that contain only a radio button question (no text inputs), the user is automatically taken to the next screen after a **300 ms** delay following their selection. No need to click "Pokračovat". Exception: on the **Proč zakládáte e-shop?** screen, selecting **Jiné** does NOT auto-advance, because it reveals a secondary optional question on the same screen. Inside that sub-question, **Jsem Shoptet partner a vytvářím e-shop pro klienta** and **Vytvářím e-shop pro klienta** auto-advance immediately. **Jiné** does NOT auto-advance — the user stays on the screen to optionally type their reason before pressing "Pokračovat".

- **Adding multiple URLs** — on the screen *Jaká je webová adresa (URL) vašeho již existujícího e-shopu? (nepovinné)*, the link **"Přidat URL dalšího e-shopu"** is only active if the first URL input field has been filled in. If the field is empty and the user clicks the link, display the error: **"Před přidáním další URL nejprve vyplňte tu první."**
