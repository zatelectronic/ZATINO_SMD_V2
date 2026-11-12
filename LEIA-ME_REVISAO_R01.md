# ZATINO SMD V2 — Revisão R01 (organização) — 2026-11-11
Zat Electronic · KiCad 10.0 · mesmo padrão do ZATINO PTH V2 R01

## O que mudou
- Esquema plano -> capa A2 + 3 folhas: 02 MCU (A3), 03 ALIMENTACAO (A4), 04 CONEXOES (A4).
- Projeto renomeado sem espaços: ZATINO_SMD_V2.kicad_pro / .kicad_sch / .kicad_pcb.
- Reanotação (ordem do KiCad, 1º livre após 0):
  REGULADOR_5V1->U2 · REGULADOR_3V1->U3 · KR1->Y1 · CONECTOR1->J2 (jack) · J2->J1 (AREF) · J1->J3 (FTDI)
  L1->LED1 · L2->LED2 · C5->C1 · C6->C2 · C7->C3 · C2->C4 · C3->C5 · C4->C6 · C1->C7 · R3->R1 · R1->R2 · R2->R3
  (U1, A1, D1, SW1, TP1 sem mudança)
- Values normalizados, Descrição PT, Stock Code, Package Group, title block Zat (Zat_TitleBlock.kicad_wks).
- U1: símbolo SnapEDA trocado por MCU_Microchip_ATmega:ATmega328PB-A (KiCad 10). Bloco refeito com rótulos globais;
  PE2 (19) e PE3 (22) com No Connect; GND 5/21 e VCC/AVCC 4/18 conforme a placa.
- D1: Diode:MRA4007T3G (SMA). J3: ZAT_Connector:Conn_01x06_Pin. A1: ZAT_Board_Arduino:ARDUINO_UNO_R3_Headers (fora da simulação).
- TP1: fora da BOM e do arquivo de posição (pad de teste, igual ao footprint KiCad).
- ERC: sem junções de 4 vias, sem fios de comprimento zero, PWR_FLAG em GND e VIN. Nenhuma severidade em "ignore".

## PCB (nenhum componente movido)
- Todos os footprints refeitos a partir da biblioteca KiCad 10 / ZAT, mesma posição e rotação; ligações por UUID.
- U1 -> ZAT_MCU_Microchip:TQFP-32_7x7mm_P0.8mm_ATmega328PB-AU (cópia do KiCad 10 + 3D ATmega_AP214).
  Pads do padrão IPC (0,55 x 1,475 mm, centro 0,14 mm mais para dentro): todas as trilhas continuam terminando
  dentro dos pads, folga entre pads 0,25 mm (>= 0,2 mm).
- SW1 -> ZAT_Button_Switch_SMD:SW_SPST_PTS645Sx43SMTR92_6x6x5mm (o SW_Push_1P1T_NO_6x6mm não existe no KiCad 10; mesmos pads).
- U2/U3 -> ZAT AMS1117-5.0/3.3 · J2 -> ZAT_Connector:BarrelJack_Horizontal_EdgeMount · J3 -> ZAT FT232RL USB-C · A1 -> ZAT headers UNO.
- Passivos, LEDs, cristal, D1, J1, TP1: biblioteca KiCad 10 com 3D padrão.
- Nets renomeadas para os nomes que o KiCad gera a partir do esquema (F8 deve vir sem alterações).
- Referências: todas visíveis em F.SilkS, 1,0 mm / 0,15 mm, fora do corpo, folgas 0,2 (silk) / 0,25 (pads) / 0,4 (borda).
- Logos ZAT/KiCad/OSHW/LEDs/polaridade: courtyard real (arte + 0,25 mm), "Not in schematic"; KiCad e OSHW viraram arte da placa.
- Arte: blocos GND refeitos sem tocar textos nem pads; contorno do FTDI afastado do J3; linhas sob DIGITAL/ANALOG IN encurtadas.
- Texto "DC IN 7V-12V": estava sobre o pad 3 do J2 e o furo de fixação -> 0,8 mm, acima do D1 (126,4 / 104,7).
- .kicad_dru só com "(version 1)" (nenhuma exceção).

## Biblioteca global (já gravada no PC)
- ZAT_MCU_Microchip.pretty/TQFP-32_7x7mm_P0.8mm_ATmega328PB-AU + 3dmodels/ZAT_MCU_ATMEGA.3dshapes/ATmega_AP214.step
- ZAT_Button_Switch_SMD.pretty/SW_SPST_PTS645Sx43SMTR92_6x6x5mm (3D ${ZAT_3D}/SWITCH/Tactal Switch - SMD 6x6x5.stp)
- fp-lib-table global: + ZAT_Button_Switch_SMD

## Pendências de projeto (exigem alteração de placa)
- IMPORTANTE: PE0 (pino 3) ligado ao GND e PE1 (pino 6) ao +5V (compatibilidade com o 328P) — nunca configurar PE0/PE1 como saída.
- IMPORTANTE: sem 100 nF nos pinos VCC (4) e AVCC (18) do ATmega328PB.
- IMPORTANTE: +5V do módulo FTDI ligado direto ao +5V da placa, sem diodo/chave (retroalimenta o AMS1117-5.0).
- ATENÇÃO: jumper J1 (AREF->3,3 V) fechado exige analogReference(EXTERNAL).
- bom/ibom.html é da versão antiga (referências antigas): gerar de novo.

## Após abrir no KiCad
1. Reiniciar o KiCad (biblioteca ZAT_Button_Switch_SMD nova).
2. ERC no esquema.
3. PCB: B (preencher zonas) -> F8 (Update PCB from Schematic: deve vir sem alterações) -> DRC.

## Correções após o ERC/DRC do Kenneth (R01.1 — 2026-11-11)
Mantidas as edições do Kenneth: textos RESET/D13/POWER/3V3/5V da borda esquerda removidos, "DC IN 7V-12V" ao lado do jack,
logo de polaridade reposicionado e trilha AREF ajustada.
- Causa dos 17 erros footprint_filter + 19 footprint_symbol_mismatch: "Update Symbols from Library" com
  "Reset ... fields" apagou o campo Footprint e trocou Descrição/Stock Code/Package Group pelos da biblioteca.
  Campos restaurados em todos os símbolos (e nos footprints do PCB, para F8 sem alterações).
- U1/U2/U3 tinham voltado para o footprint padrão (sem o 3D ZAT): de volta para os ZAT_ (mesmos pads).
- Logos KiCad/OSHW/WEEE tinham voltado para a biblioteca (sem courtyard e sem "Not in schematic"):
  board_only + courtyard de novo (fim de missing_courtyard, duplicate_footprints e extra_footprint).
- Referências na serigrafia recolocadas (o update de footprint tinha voltado todas para a posição da biblioteca).
- Verso: texto MODEL/HARDWARE/INSTAGRAM 2,5 mm para a esquerda (encostava no logo WEEE).
- TP1 de volta à grade de 1,27 mm (endpoint_off_grid) e fora da BOM/CPL.
- PE0 (3) e PE1 (6) desligados dos trilhos (autorizado): No Connect no esquema; no PCB saiu o trecho de trilha
  pino 6 -> nó +5V e o pino 3 não pega mais a zona GND. Firmware: pull-up interno em PE0/PE1.

### Atenção ao usar "Update Symbols from Library" / "Update Footprints from Library"
- Símbolos: NÃO marcar "Reset ... fields" (apaga Footprint, Descrição, Stock Code, Package Group).
- Footprints: NÃO marcar "Reset text/field positions" nem atualizar os logos (perdem courtyard e board_only).
