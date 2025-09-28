---
layout: post
title: Baccarat Game on FPGA
description: Designed and synthesized Baccarat game engine on DE1-Soc FPGA using Quartus, ModelSim, and System Verilog. 
skills: 
  - FPGA
  - Quartus
  - ModelSim
  - SystemVerilog

main-image: 
---
## Game Rules
<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Two cards are dealt to both the player and the dealer (banker), face up in order: first to the player, second to the dealer, third to the player, and fourth to the dealer</li>
  <li>The score of each hand is computed as described under <b>Score</b> below</li>
  <li>If the player’s or banker’s hand has a score of 8 or 9, the game ends immediately (a “natural”); the higher score wins, or it is a tie if scores are equal</li>
  <li>Otherwise, if the player’s first two cards total 0–5:
    <ul>
      <li>The player receives a third card</li>
      <li>The banker may also receive a third card depending on the following rules:
        <ul>
          <li>If banker’s score is 7 → banker stands</li>
          <li>If banker’s score is 6 → banker draws if player’s third card is 6 or 7</li>
          <li>If banker’s score is 5 → banker draws if player’s third card is 4, 5, 6, or 7</li>
          <li>If banker’s score is 4 → banker draws if player’s third card is 2–7</li>
          <li>If banker’s score is 3 → banker draws unless player’s third card is 8</li>
          <li>If banker’s score is 0–2 → banker always draws</li>
        </ul>
      </li>
    </ul>
  </li>
  <li>Otherwise, if the player’s first two cards total 6 or 7:
    <ul>
      <li>The player stands (does not take a third card)</li>
      <li>If banker’s score is 0–5 → banker draws, otherwise banker stands</li>
    </ul>
  </li>
  <li>The game ends. Scores are compared; the higher score wins, or it is a tie if equal</li>
</ul>

<h3 style="color: #e0e0e0;">Score</h3>
<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Each Ace, 2–9 has a value equal to its face value (Ace = 1, 2 = 2, … 9 = 9)</li>
  <li>Tens, Jacks, Queens, and Kings each have a value of 0</li>
  <li>The score of a hand (2 or 3 cards) is the sum of the card values modulo 10:
    <br><b>Score = (Value1 + Value2 + Value3) mod 10</b></li>
  <li>If the hand has only two cards, treat Value3 as 0</li>
  <li>Scores are always in the range 0–9</li>
</ul>


## Main features 
<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Designed and synthesized digital datapath and finite state machine controller to implement Baccarat game engine on DE1-Soc FPGA using Quartus and SystemVerilog</li>
  <li>Controlled game cards using push buttons, LED’s, and 7-sgement display while keeping track of score and player hands using registers</li>
  <li>Built hierarchal modules and wrote extensive ModelSim test benches for RTL and post-synthesis to verify functionality, specifically testing edge cases</li>
</ul>

## Project Demo 

<iframe src="https://drive.google.com/file/d/1bhns0-bulK6TT5thMRLraTc1YyPzXWgW/view?usp=drive_link" 
        width="640" height="360" allow="autoplay"></iframe>
