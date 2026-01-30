# Vaja5-buttons-LED-STM32F0

## Opis projekta
Cilj naloge je bil sprogramirati mikrokrmilnik tako, da s pomočjo tipk krmilimo utripanje zelene in modre LED diode na različne načine (frekvenca, Morsejeva abeceda, pospeševanje/upočasnjevanje).

Projekt je bil prvotno zasnovan za STM32F0, vendar je v tej izvedbi prilagojen za razvojno ploščo **STM32L100C-Discovery**.

## Uporabljena strojna oprema
* **Razvojna plošča:** STM32L100C-Discovery (STM32L100RCT6)
* **Razširitveno vezje:** Baseboard s tipkami

## 1. Pinout in inicializacija (Odgovori na vprašanja)
Spodaj so navedeni uporabljeni pini za tipke in LED diode, kot zahteva točka 2b v navodilih.

| Komponenta | Pin na STM32L1 | Opomba |
| :--- | :--- | :--- |
| **Tipka T1** | PB10 | Zelena LED, 2 Hz, 5x |
| **Tipka T2** | PB11 | Modra LED, 2 Hz, 5x |
| **Tipka T3** | PB12 | Izmenično utripanje (1 Hz) |
| **Tipka T4** | PB13 | SOS (Morse) |
| **Tipka T5** | PB14 | Zelena LED (pospeševanje) |
| **Tipka T6** | PB15 | Izmenično (upočasnjevanje) |
| **Zelena LED**| PC9 (LD3) | Vgrajena na plošči |
| **Modra LED** | PC8 (LD4) | Vgrajena na plošči |

### Slika Pinout-a (CubeMX)
<img width="729" height="616" alt="image" src="https://github.com/user-attachments/assets/294c1d78-b86a-4f44-86cd-93802d855209" />


## 2. Opis delovanja (Komentar)
Program se izvaja v neskončni zanki (`while(1)`), kjer mikrokrmilnik neprestano preverja stanje digitalnih vhodov (tipk T1–T6).

* **Logika tipk:** Uporabljena je "polling" metoda. Ko je zaznan pritisk tipke (logična 1), se izvede ustrezna `if` zanka s specifičnim zaporedjem utripanja.
* **Preprečevanje ponovitev:** Na koncu vsakega bloka je dodana `while` zanka, ki čaka, da uporabnik spusti tipko, preden program nadaljuje. S tem preprečimo večkratno proženje iste funkcije ob enem pritisku.
* **Časovne zakasnitve:** Za generiranje frekvenc in časovnih intervalov je uporabljena funkcija `HAL_Delay()`.
* **SOS signal:** Za SOS signal (T4) sta implementirani pomožni funkciji `blink_dot()` in `blink_dash()`, ki poenostavita kodo za Morsejevo abecedo.
* **Spreminjanje frekvence (T5 in T6):** Frekvenca se dinamično spreminja s spreminjanjem parametra v funkciji `HAL_Delay` znotraj `while` zanke (deljenje ali množenje zakasnitve z 2).

## 3. Video delovanja
<a href="https://drive.google.com/file/d/13MaKI4YX3ek_r8ZQQt8gGzEf8y_3zF4f/view?usp=sharing">Posnetek delovanja</a>
