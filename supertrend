<?php

function trader_supertrend($data, $period, $multiplier) {
    $previous_final_upperband = 0;
    $previous_final_lowerband = 0;
    $final_upperband = 0;
    $final_lowerband = 0;
    $previous_close = 0;
    $previous_supertrend = 0;
    $supertrend = [];
    $supertrendc = 0;

    $history = [];

    $trader_atr = trader_atr($data['high'], $data['low'],$data['close'],$period);
    foreach ($trader_atr as $index => $atr) {
        if (is_nan($atr)) $atr = 0;
        $closec = $data['close'][$index];

        $basic_upperband = ($data['high'][$index] + $data['low'][$index]) / 2 + $multiplier * $atr;
        $basic_lowerband = ($data['high'][$index] + $data['low'][$index]) / 2 - $multiplier * $atr;
        ///////////////////////////////////////

        /*"""
    SuperTrend Algorithm :

        BASIC UPPERBAND = (HIGH + LOW) / 2 + Multiplier * ATR
        BASIC LOWERBAND = (HIGH + LOW) / 2 - Multiplier * ATR

        FINAL UPPERBAND = IF( (Current BASICUPPERBAND < Previous FINAL UPPERBAND) or (Previous Close > Previous FINAL UPPERBAND))
                            THEN (Current BASIC UPPERBAND) ELSE Previous FINALUPPERBAND)
        FINAL LOWERBAND = IF( (Current BASIC LOWERBAND > Previous FINAL LOWERBAND) or (Previous Close < Previous FINAL LOWERBAND))
                            THEN (Current BASIC LOWERBAND) ELSE Previous FINAL LOWERBAND)

        SUPERTREND = IF((Previous SUPERTREND = Previous FINAL UPPERBAND) and (Current Close <= Current FINAL UPPERBAND)) THEN
                        Current FINAL UPPERBAND
                    ELSE
                        IF((Previous SUPERTREND = Previous FINAL UPPERBAND) and (Current Close > Current FINAL UPPERBAND)) THEN
                            Current FINAL LOWERBAND
                        ELSE
                            IF((Previous SUPERTREND = Previous FINAL LOWERBAND) and (Current Close >= Current FINAL LOWERBAND)) THEN
                                Current FINAL LOWERBAND
                            ELSE
                                IF((Previous SUPERTREND = Previous FINAL LOWERBAND) and (Current Close < Current FINAL LOWERBAND)) THEN
                                    Current FINAL UPPERBAND
    """*/

            if (($basic_upperband < $previous_final_upperband) or ($previous_close > $previous_final_upperband)){
                    $final_upperband = $basic_upperband;
                } else {
                    $final_upperband = $previous_final_upperband;
            }
            if ($basic_lowerband > $previous_final_lowerband or $previous_close < $previous_final_lowerband) {
                    $final_lowerband = $basic_lowerband;
                } else {
                    $final_lowerband = $previous_final_lowerband;
            }




            if ($previous_supertrend == $previous_final_upperband and $closec <= $final_upperband) {
                    $supertrendc = $final_upperband;
                } else {
                    if ($previous_supertrend == $previous_final_upperband and $closec >= $final_upperband) {
                        $supertrendc = $final_lowerband;
                    } else {
                        if ($previous_supertrend == $previous_final_lowerband and $closec >= $final_lowerband) {
                            $supertrendc = $final_lowerband;
                        } elseif ($previous_supertrend == $previous_final_lowerband and $closec <= $final_lowerband) {
                            $supertrendc = $final_upperband;
                        }
                    }
            }

            $supertrend[$index]       = [
                'close' => $supertrendc,
                'type'  => ($supertrendc==$final_upperband?'sell':'buy')
            ];

            $previous_close           = $closec;
            $previous_final_upperband = $final_upperband;
            $previous_final_lowerband = $final_lowerband;
            $previous_supertrend      = $supertrendc;



    }

    return $supertrend;
}


$data = ['close'=>[....],'low'=>[....],'high'=>[....],'open'=>[....]]; 
$period = 10;
$multiplier = 3;
$supertrend = trader_supertrend($data, $period, $multiplier);
