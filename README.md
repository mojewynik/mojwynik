<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ekstraklasa - Tabela Ligowa</title>
    <style>
        /* Reset podstawowych marginesów */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            color: #333;
        }

        /* GŁÓWNY CIEMNY PASEK */
        .top-bar {
            background: linear-gradient(180deg, #001d42, #00142d);
            color: white;
            text-align: center;
            padding: 22px 10px;
            box-shadow: inset 0 1px 0 rgba(255,255,255,0.1);
        }

        .top-bar h1 {
            font-size: 1.55rem;
            font-weight: 700;
            letter-spacing: 0.3px;
            margin-bottom: 5px;
        }

        .top-bar h1 span {
            font-size: 0.95rem;
            font-weight: 400;
            opacity: 0.85;
            margin-right: 6px;
            text-transform: uppercase;
        }

        .top-bar p {
            font-size: 0.78rem;
            opacity: 0.65;
            font-weight: 300;
            letter-spacing: 0.2px;
        }

        /* MNIEJSZY PASEK Z WYBOREM LIG */
        .league-bar {
            background-color: #ffffff;
            border-bottom: 1px solid #dee2e6;
            display: flex;
            justify-content: center;
            gap: 15px;
            padding: 10px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
        }

        .league-btn {
            background: none;
            border: none;
            color: #495057;
            font-size: 0.9rem;
            font-weight: 600;
            cursor: pointer;
            padding: 6px 14px;
            border-radius: 20px;
            transition: all 0.2s ease;
        }

        .league-btn:hover {
            background-color: #e9ecef;
            color: #001d42;
        }

        .league-btn.active {
            background-color: #001d42;
            color: #ffffff;
        }

        /* TWOJE NOWE STYLE KLAS TABELI I ELEMENTÓW */
        main {
            max-width: 900px;
            margin: 30px auto;
            padding: 0 15px;
        }
        .liga-section {
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
            padding: 24px;
        }
        .ekstraklasa-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            font-size: 0.95em;
            background-color: white;
            overflow: hidden;
            border-radius: 8px;
            border: 1px solid #eef0f5;
        }
        .ekstraklasa-table th {
            background-color: #002f6c;
            color: white;
            font-weight: 600;
            padding: 12px 10px;
            text-align: center;
        }
        .ekstraklasa-table th.left-align, .ekstraklasa-table td.left-align {
            text-align: left;
            padding-left: 15px;
        }
        .ekstraklasa-table td {
            padding: 12px 10px;
            text-align: center;
            border-bottom: 1px solid #edf0f4;
            color: #444;
        }
        .ekstraklasa-table tbody tr:hover {
            background-color: #f7f9fc;
            transition: background-color 0.2s ease;
        }
        
        /* STREFY COLOURS (1-4 niebieskie, 16-18 czerwone) */
        .zone-blue { border-left: 5px solid #1565c0; background-color: rgba(21, 101, 192, 0.01); }
        .zone-relegation { border-left: 5px solid #c62828; background-color: rgba(198, 40, 40, 0.02); }
        
        .pos-num { font-weight: bold; }
        .zone-blue .pos-num { color: #1565c0; }
        .zone-relegation .pos-num { color: #c62828; }
        
        .team-name { font-weight: 600; color: #222; }
        .pts-cell { font-weight: bold; color: #002f6c; background-color: rgba(0, 47, 108, 0.04); }

        .legend {
            display: flex;
            gap: 20px;
            margin-top: 20px;
            font-size: 0.85em;
            color: #666;
            flex-wrap: wrap;
        }
        .legend-item { display: flex; align-items: center; gap: 6px; }
        .dot { width: 10px; height: 10px; border-radius: 50%; }
        .dot-blue { background-color: #1565c0; }
        .dot-red { background-color: #c62828; }

    </style>
</head>
<body>

<!-- Główny dynamiczny pasek z dodanym logo ligi -->
<div class="top-bar">
    <div style="display: flex; align-items: center; justify-content: center; gap: 15px;">
        <!-- To logo będzie się zmieniać dynamicznie dzięki skryptowi -->
        <img id="league-logo" src="herby/logo-ekstraklasa.png" alt="" style="height: 45px; width: auto; vertical-align: middle;">
        <div>
            <h1 id="main-title"><span id="country-tag">PL</span> PKO BP Ekstraklasa</h1>
            <p id="sub-title">Oficjalna tabela sezonu 2026/2027</p>
        </div>
    </div>
</div>


<!-- Pasek wyboru lig -->
<div class="league-bar">
    <button class="league-btn active" onclick="selectLeague('PL', 'PKO BP Ekstraklasa', 'Oficjalna tabela sezonu 2026/2027', this, true)">Ekstraklasa</button>
    <button class="league-btn" onclick="selectLeague('ES', 'La Liga', 'Oficjalna tabela sezonu 2026/2027', this, false)">La Liga</button>
    <button class="league-btn" onclick="selectLeague('UK', 'Premier League', 'Oficjalna tabela sezonu 2026/2027', this, false)">Premier League</button>
</div>



            <div class="liga-section">
<!-- ========================================================================= -->
<!-- POCZĄTEK FRAGMENTU: Profesjonalny Terminarz z podziałem na dni i herby   -->
<!-- ========================================================================= -->
<div class="header-row" style="display: flex; align-items: center; justify-content: space-between; border-bottom: 2px solid #cbd5e1; padding-bottom: 10px; margin-bottom: 15px; font-family: 'Segoe UI', Helvetica, sans-serif;">
    <h2 style="margin: 0; font-size: 18px; color: #002d72; font-weight: bold;">Wybierz kolejkę:</h2>
    
    <select id="kolejkaSelect" onchange="zmienKolejkeEkstraklasa()" style="padding: 6px 12px; border: 1px solid #cbd5e1; border-radius: 4px; font-size: 14px; font-weight: bold; cursor: pointer; outline: none; background-color: #fff;">
        <option value="1">1. Kolejka</option>
        <option value="2">2. Kolejka</option>
        <option value="3">3. Kolejka</option>
        <option value="4">4. Kolejka</option>
        <option value="5">5. Kolejka</option>
        <option value="6">6. Kolejka</option>
        <option value="7">7. Kolejka</option>
        <option value="8">8. Kolejka</option>
        <option value="9">9. Kolejka</option>
        <option value="10">10. Kolejka</option>
        <option value="11">11. Kolejka</option>
        <option value="12">12. Kolejka</option>
        <option value="13" selected>13. Kolejka (Aktualna)</option>
        <option value="14">14. Kolejka</option>
        <option value="15">15. Kolejka</option>
        <option value="16">16. Kolejka</option>
        <option value="17">17. Kolejka</option>
        <option value="18">18. Kolejka</option>
        <option value="19">19. Kolejka</option>
        <option value="20">20. Kolejka</option>
        <option value="21">21. Kolejka</option>
        <option value="22">22. Kolejka</option>
        <option value="23">23. Kolejka</option>
        <option value="24">24. Kolejka</option>
        <option value="25">25. Kolejka</option>
        <option value="26">26. Kolejka</option>
        <option value="27">27. Kolejka</option>
        <option value="28">28. Kolejka</option>
        <option value="29">29. Kolejka</option>
        <option value="30">30. Kolejka</option>
        <option value="31">31. Kolejka</option>
        <option value="32">32. Kolejka</option>
        <option value="33">33. Kolejka</option>
        <option value="34">34. Kolejka</option>
    </select>
</div>

<div id="kontenerMeczowEkstraklasa" style="font-family: 'Segoe UI', Helvetica, sans-serif; margin-bottom: 25px;"></div>


<script>
// Prosta struktura danych - dopisujesz mecz po meczu, a JS sam pogrupuje je według "status"
const meczeEkstraklasy = {
    1: [
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Piątek, 24 lipca 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Piątek, 24 lipca 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Sobota, 25 lipca 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Sobota, 25 lipca 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Sobota, 25 lipca 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Niedziela, 26 lipca 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Niedziela, 26 lipca 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Niedziela, 26 lipca 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Poniedziałek, 27 lipca 2026" }
],

2: [
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Piątek, 31 lipca 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Piątek, 31 lipca 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Sobota, 1 sierpnia 2026" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Sobota, 1 sierpnia 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Niedziela, 2 sierpnia 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Niedziela, 2 sierpnia 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Niedziela, 2 sierpnia 2026" },
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Poniedziałek, 3 sierpnia 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Wtorek, 15 września 2026" }
],

3: [
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Piątek, 7 sierpnia 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Sobota, 8 sierpnia 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Sobota, 8 sierpnia 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Sobota, 8 sierpnia 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Niedziela, 9 sierpnia 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Niedziela, 9 sierpnia 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Niedziela, 9 sierpnia 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Wtorek, 15 września 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Poniedziałek, 12 października 2026" }
],

4: [
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Piątek, 14 sierpnia 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Sobota, 15 sierpnia 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Sobota, 15 sierpnia 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Sobota, 15 sierpnia 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Niedziela, 16 sierpnia 2026" },
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Niedziela, 16 sierpnia 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Niedziela, 16 sierpnia 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Środa, 16 grudnia 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Czwartek, 17 grudnia 2026" }
],

5: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Piątek, 21 sierpnia 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Sobota, 22 sierpnia 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Sobota, 22 sierpnia 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Sobota, 22 sierpnia 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Niedziela, 23 sierpnia 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Niedziela, 23 sierpnia 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Niedziela, 23 sierpnia 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Czwartek, 3 września 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Czwartek, 3 września 2026" }
],

6: [
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Piątek, 28 sierpnia 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Piątek, 28 sierpnia 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Sobota, 29 sierpnia 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Sobota, 29 sierpnia 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Sobota, 29 sierpnia 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Sobota, 29 sierpnia 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Niedziela, 30 sierpnia 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Niedziela, 30 sierpnia 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Niedziela, 30 sierpnia 2026" }
],

7: [
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Piątek, 4 września 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Sobota, 5 września 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Sobota, 5 września 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Sobota, 5 września 2026" },
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Niedziela, 6 września 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Niedziela, 6 września 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Niedziela, 6 września 2026" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Poniedziałek, 7 września 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Poniedziałek, 7 września 2026" }
],

8: [
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Piątek, 11 września 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Piątek, 11 września 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Sobota, 12 września 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Sobota, 12 września 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Sobota, 12 września 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Niedziela, 13 września 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Niedziela, 13 września 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Niedziela, 13 września 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Poniedziałek, 14 września 2026" }
],

9: [
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Piątek, 18 września 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Piątek, 18 września 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Sobota, 19 września 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Sobota, 19 września 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Sobota, 19 września 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Niedziela, 20 września 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Niedziela, 20 września 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Niedziela, 20 września 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Niedziela, 20 września 2026" }
],

10: [
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Piątek, 9 października 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Piątek, 9 października 2026" },
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Sobota, 10 października 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Sobota, 10 października 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Sobota, 10 października 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Niedziela, 11 października 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Niedziela, 11 października 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Niedziela, 11 października 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Poniedziałek, 12 października 2026" }
],

11: [
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Piątek, 16 października 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Piątek, 16 października 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Sobota, 17 października 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Sobota, 17 października 2026" },
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Sobota, 17 października 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Niedziela, 18 października 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Niedziela, 18 października 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Niedziela, 18 października 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Poniedziałek, 19 października 2026" }
],

12: [
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Piątek, 23 października 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Piątek, 23 października 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Sobota, 24 października 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Sobota, 24 października 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Sobota, 24 października 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Sobota, 24 października 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Niedziela, 25 października 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Niedziela, 25 października 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Niedziela, 25 października 2026" }
],
13: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Piątek, 30 października 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Piątek, 30 października 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Sobota, 31 października 2026" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Sobota, 31 października 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Sobota, 31 października 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Sobota, 31 października 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Poniedziałek, 2 listopada 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Poniedziałek, 2 listopada 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Poniedziałek, 2 listopada 2026" }
],

14: [
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Piątek, 6 listopada 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Piątek, 6 listopada 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Sobota, 7 listopada 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Sobota, 7 listopada 2026" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Sobota, 7 listopada 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Sobota, 7 listopada 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Niedziela, 8 listopada 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Niedziela, 8 listopada 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Niedziela, 8 listopada 2026" }
],

15: [
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Piątek, 20 listopada 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Piątek, 20 listopada 2026" },
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Sobota, 21 listopada 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Sobota, 21 listopada 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Sobota, 21 listopada 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Niedziela, 22 listopada 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Niedziela, 22 listopada 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Niedziela, 22 listopada 2026" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Poniedziałek, 23 listopada 2026" }
],

16: [
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "00:00", status: "Sobota, 28 listopada 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "00:00", status: "Sobota, 28 listopada 2026" }
],

17: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "00:00", status: "Sobota, 5 grudnia 2026" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Sobota, 5 grudnia 2026" }
],

18: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "00:00", status: "Sobota, 12 grudnia 2026" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "00:00", status: "Sobota, 12 grudnia 2026" }
],
19: [
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 30 stycznia 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 30 stycznia 2027" }
],

20: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 6 lutego 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 6 lutego 2027" }
],

21: [
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 13 lutego 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Sobota, 13 lutego 2027" }
],

22: [
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 20 lutego 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 20 lutego 2027" }
],

23: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 27 lutego 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 27 lutego 2027" }
],

24: [
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 6 marca 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Sobota, 6 marca 2027" }
],

25: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 13 marca 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 13 marca 2027" }
],

26: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 20 marca 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 20 marca 2027" }
],

27: [
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 3 kwietnia 2027" }
],

28: [
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 10 kwietnia 2027" }
],

29: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 17 kwietnia 2027" }
],

30: [
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Piątek, 23 kwietnia 2027" }
],

31: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 1 maja 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 1 maja 2027" }
],

32: [
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 8 maja 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 8 maja 2027" }
],

33: [
    { home: "Cracovia", homeLogo: "herby/cracovia.png", away: "Korona Kielce", awayLogo: "herby/korona.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "GKS Katowice", homeLogo: "herby/gks-katowice.png", away: "Śląsk Wrocław", awayLogo: "herby/slask.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Motor Lublin", homeLogo: "herby/motor.png", away: "Wisła Kraków", awayLogo: "herby/wisla-krakow.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Piast Gliwice", homeLogo: "herby/piast.png", away: "Raków Częstochowa", awayLogo: "herby/rakow.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Pogoń Szczecin", homeLogo: "herby/pogon.png", away: "Lech Poznań", awayLogo: "herby/lech.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Radomiak Radom", homeLogo: "herby/radomiak.png", away: "Jagiellonia Białystok", awayLogo: "herby/jagiellonia.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Wieczysta Kraków", homeLogo: "herby/wieczysta.png", away: "Legia Warszawa", awayLogo: "herby/legia.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Wisła Płock", homeLogo: "herby/wisla-plock.png", away: "Górnik Zabrze", awayLogo: "herby/gornik.png", score: "18:00", status: "Sobota, 15 maja 2027" },
    { home: "Zagłębie Lubin", homeLogo: "herby/zaglebie.png", away: "Widzew Łódź", awayLogo: "herby/widzew.png", score: "18:00", status: "Sobota, 15 maja 2027" }
],

34: [
    { home: "Górnik Zabrze", homeLogo: "herby/gornik.png", away: "Pogoń Szczecin", awayLogo: "herby/pogon.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Jagiellonia Białystok", homeLogo: "herby/jagiellonia.png", away: "Piast Gliwice", awayLogo: "herby/piast.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Korona Kielce", homeLogo: "herby/korona.png", away: "Wieczysta Kraków", awayLogo: "herby/wieczysta.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Lech Poznań", homeLogo: "herby/lech.png", away: "Motor Lublin", awayLogo: "herby/motor.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Legia Warszawa", homeLogo: "herby/legia.png", away: "GKS Katowice", awayLogo: "herby/gks-katowice.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Raków Częstochowa", homeLogo: "herby/rakow.png", away: "Radomiak Radom", awayLogo: "herby/radomiak.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Śląsk Wrocław", homeLogo: "herby/slask.png", away: "Wisła Płock", awayLogo: "herby/wisla-plock.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Widzew Łódź", homeLogo: "herby/widzew.png", away: "Cracovia", awayLogo: "herby/cracovia.png", score: "18:00", status: "Sobota, 22 maja 2027" },
    { home: "Wisła Kraków", homeLogo: "herby/wisla-krakow.png", away: "Zagłębie Lubin", awayLogo: "herby/zaglebie.png", score: "18:00", status: "Sobota, 22 maja 2027" }
]
};

function zmienKolejkeEkstraklasa() {
    const select = document.getElementById("kolejkaSelect");
    const wybranyNumer = select.value;
    const kontener = document.getElementById("kontenerMeczowEkstraklasa");
    
    kontener.innerHTML = "";
    const mecze = meczeEkstraklasy[wybranyNumer];
    if (!mecze) return;

    // Automatyczne grupowanie meczów po polu "status"
    const pogrupowaneMecze = {};
    mecze.forEach(mecz => {
        if (!pogrupowaneMecze[mecz.status]) {
            pogrupowaneMecze[mecz.status] = [];
        }
        pogrupowaneMecze[mecz.status].push(mecz);
    });

    // Renderowanie wygenerowanych grup
    Object.keys(pogrupowaneMecze).forEach(nazwaStatusu => {
        // Pasek nagłówka grupy
        const pasekDnia = document.createElement("div");
        pasekDnia.style.cssText = "background-color: #003366; color: #ffffff; padding: 10px 15px; font-weight: bold; font-size: 15px; margin-top: 12px; border-radius: 4px 4px 0 0;";
        pasekDnia.innerText = nazwaStatusu;
        kontener.appendChild(pasekDnia);

        // Kontener na mecze z danej grupy
        const listaMeczow = document.createElement("div");
        listaMeczow.style.cssText = "border: 1px solid #cbd5e1; border-top: none; background: #ffffff; border-radius: 0 0 4px 4px; margin-bottom: 5px;";

        pogrupowaneMecze[nazwaStatusu].forEach(mecz => {
            const row = document.createElement("div");
            row.style.cssText = "display: flex; align-items: center; padding: 14px 10px; border-bottom: 1px solid #f1f5f9;";
            
            // Funkcja sprawdzająca czy wpisano wynik (zawiera dwukropek ze spacjami lub bez), czy samą godzinę
            const czyWynik = mecz.score.includes(":") && !mecz.score.match(/^[0-2][0-9]:[0-5][0-9]$/);
            
            const scoreStyle = czyWynik 
                ? "width: 55px; background: #002d72; color: #ffffff; text-align: center; font-weight: bold; padding: 5px 0; border-radius: 4px; font-size: 14px;"
                : "width: 55px; background: #f1f5f9; color: #64748b; text-align: center; font-weight: 500; padding: 5px 0; border-radius: 4px; font-size: 13px; border: 1px solid #e2e8f0;";

            row.innerHTML = `
                <div style="flex: 1; text-align: right; font-weight: 600; color: #1e293b; padding-right: 8px; font-size: 14px;">${mecz.home}</div>
                <img src="${mecz.homeLogo}" alt="" style="width: 22px; height: 22px; object-fit: contain; margin-right: 12px;" onerror="this.style.opacity='0'">
                
                <div style="${scoreStyle}">${mecz.score}</div>
                
                <img src="${mecz.awayLogo}" alt="" style="width: 22px; height: 22px; object-fit: contain; margin-left: 12px;" onerror="this.style.opacity='0'">
                <div style="flex: 1; text-align: left; font-weight: 600; color: #1e293b; padding-left: 8px; font-size: 14px;">${mecz.away}</div>
            `;
            listaMeczow.appendChild(row);
        });

        kontener.appendChild(listaMeczow);
    });
}

// Inicjalizacja przy ładowaniu strony
document.addEventListener("DOMContentLoaded", zmienKolejkeEkstraklasa);
</script>

<!-- ========================================================================= -->
<!-- KONIEC FRAGMENTU                                                          -->
<!-- ========================================================================= -->

            <h2>📊 Tabela Ligowa</h2>
            
            <table class="ekstraklasa-table">
                <thead>
                    <tr>
                        <th style="width: 50px;">Poz</th>
                        <th class="left-align">Klub</th>
                        <th style="width: 60px;">Mecze</th>
                        <th style="width: 100px;">Bramki</th>
                        <th style="width: 70px;">Pkt</th>
                    </tr>
                </thead>
                <tbody id="tabela-pozycji">
                    <!-- 1 -->
                    <tr class="zone-blue">
                        <td class="pos-num">1</td>
                        <td class="left-align team-name">
                            <img src="herby/gornik.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Górnik Zabrze
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 2 -->
                    <tr class="zone-blue">
                        <td class="pos-num">2</td>
                        <td class="left-align team-name">
                            <img src="herby/lech.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Lech Poznań
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 3 -->
                    <tr class="zone-blue">
                        <td class="pos-num">3</td>
                        <td class="left-align team-name">
                            <img src="herby/legia.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Legia Warszawa
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 4 -->
                    <tr class="zone-blue">
                        <td class="pos-num">4</td>
                        <td class="left-align team-name">
                            <img src="herby/pogon.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Pogoń Szczecin
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 5 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">5</td>
                        <td class="left-align team-name">
                            <img src="herby/zaglebie.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Zagłębie Lubin
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 6 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">6</td>
                        <td class="left-align team-name">
                            <img src="herby/gks-katowice.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">GKS Katowice
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 7 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">7</td>
                        <td class="left-align team-name">
                            <img src="herby/wisla-krakow.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Wisła Kraków
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 8 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">8</td>
                        <td class="left-align team-name">
                            <img src="herby/piast.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Piast Gliwice
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 9 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">9</td>
                        <td class="left-align team-name">
                            <img src="herby/korona.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Korona Kielce
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 10 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">10</td>
                        <td class="left-align team-name">
                            <img src="herby/wisla-plock.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Wisła Płock
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 11 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">11</td>
                        <td class="left-align team-name">
                            <img src="herby/motor.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Motor Lublin
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 12 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">12</td>
                        <td class="left-align team-name">
                            <img src="herby/cracovia.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Cracovia
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 13 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">13</td>
                        <td class="left-align team-name">
                            <img src="herby/jagiellonia.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Jagiellonia Białystok
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 14 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">14</td>
                        <td class="left-align team-name">
                            <img src="herby/widzew.png" alt="🔴" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Widzew Łódź
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 15 -->
                    <tr>
                        <td class="pos-num" style="color: #666;">15</td>
                        <td class="left-align team-name">
                            <img src="herby/wieczysta.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Wieczysta Kraków
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 16 (Strefa spadkowa) -->
                    <tr class="zone-relegation">
                        <td class="pos-num">16</td>
                        <td class="left-align team-name">
                            <img src="herby/radomiak.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Radomiak Radom
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 17 (Strefa spadkowa) -->
                    <tr class="zone-relegation">
                        <td class="pos-num">17</td>
                        <td class="left-align team-name">
                            <img src="herby/slask.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Śląsk Wrocław
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <!-- 18 (Strefa spadkowa) -->
                    <tr class="zone-relegation">
                        <td class="pos-num">18</td>
                        <td class="left-align team-name">
                            <img src="herby/rakow.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Raków Częstochowa
                        </td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>

            </tbody>
        </table>
        <div class="legend">
            <div class="legend-item"><div class="dot dot-blue"></div> Strefa pucharowa</div>
            <div class="legend-item"><div class="dot dot-red"></div> Strefa spadkowa</div>
        </div>
    </div>
<script>
// Baza danych dla La Liga
const meczeLaLiga = {
    1: [
        { home: "Deportivo Alavés", homeLogo: "herby/alaves.png", away: "Getafe CF", awayLogo: "herby/getafe.png", score: "3 : 0", status: "Mecze rozegrane" },
        { home: "Sevilla FC", homeLogo: "herby/sevilla.png", away: "Rayo Vallecano", awayLogo: "herby/rayo.png", score: "2 : 1", status: "Mecze rozegrane" }
    ],
    11: [
        { home: "Villarreal CF", homeLogo: "herby/villarreal.png", away: "RCD Espanyol", awayLogo: "herby/espanyol.png", score: "0 : 1", status: "Piątek, 30 października (Zakończone)" },
        { home: "Levante UD", homeLogo: "herby/levante.png", away: "Atlético Madryt", awayLogo: "herby/atletico.png", score: "14:00", status: "Sobota, 31 października" },
        { home: "Rayo Vallecano", homeLogo: "herby/rayo.png", away: "Celta Vigo", awayLogo: "herby/celta.png", score: "16:15", status: "Sobota, 31 października" },
        { home: "Real Betis", homeLogo: "herby/betis.png", away: "Málaga CF", awayLogo: "herby/malaga.png", score: "18:30", status: "Sobota, 31 października" },
        { home: "FC Barcelona", homeLogo: "herby/barcelona.png", away: "Deportivo Alavés", awayLogo: "herby/alaves.png", score: "21:00", status: "Sobota, 31 października" },
        { home: "Getafe CF", homeLogo: "herby/getafe.png", away: "Sevilla FC", awayLogo: "herby/sevilla.png", score: "14:00", status: "Niedziela, 1 listopada" },
        { home: "Racing Santander", homeLogo: "herby/racing.png", away: "Real Madryt", awayLogo: "herby/real.png", score: "16:15", status: "Niedziela, 1 listopada" }
    ]
};

function zmienKolejkeLaLiga() {
    const select = document.getElementById("kolejkaLaLigaSelect");
    if (!select) return;
    const wybranyNumer = select.value;
    const kontener = document.getElementById("kontenerMeczowLaLiga");
    
    kontener.innerHTML = "";
    const mecze = meczeLaLiga[wybranyNumer];
    if (!mecze) return;

    const pogrupowane = {};
    mecze.forEach(mecz => {
        if (!pogrupowane[mecz.status]) {
            pogrupowane[mecz.status] = [];
        }
        pogrupowane[mecz.status].push(mecz);
    });

    Object.keys(pogrupowane).forEach(status => {
        const pasekDnia = document.createElement("div");
        pasekDnia.style.cssText = "background-color: #7a1c1c; color: #ffffff; padding: 10px 15px; font-weight: bold; font-size: 15px; margin-top: 12px; border-radius: 4px 4px 0 0;";
        pasekDnia.innerText = status;
        kontener.appendChild(pasekDnia);

        const listaMeczow = document.createElement("div");
        listaMeczow.style.cssText = "border: 1px solid #cbd5e1; border-top: none; background: #ffffff; border-radius: 0 0 4px 4px; margin-bottom: 5px;";

        pogrupowane[status].forEach(mecz => {
            const row = document.createElement("div");
            row.style.cssText = "display: flex; align-items: center; padding: 14px 10px; border-bottom: 1px solid #f1f5f9;";
            
            const czyWynik = mecz.score.includes(":") && !mecz.score.match(/^[0-2][0-9]:[0-5][0-9]$/);
            
            const scoreStyle = czyWynik 
                ? "width: 55px; background: #7a1c1c; color: #ffffff; text-align: center; font-weight: bold; padding: 5px 0; border-radius: 4px; font-size: 14px;"
                : "width: 55px; background: #f1f5f9; color: #64748b; text-align: center; font-weight: 500; padding: 5px 0; border-radius: 4px; font-size: 13px; border: 1px solid #e2e8f0;";

            row.innerHTML = `
                <div style="flex: 1; text-align: right; font-weight: 600; color: #1e293b; padding-right: 8px; font-size: 14px;">${mecz.home}</div>
                <img src="${mecz.homeLogo}" alt="" style="width: 22px; height: 22px; object-fit: contain; margin-right: 12px;" onerror="this.style.opacity='0'">
                <div style="${scoreStyle}">${mecz.score}</div>
                <img src="${mecz.awayLogo}" alt="" style="width: 22px; height: 22px; object-fit: contain; margin-left: 12px;" onerror="this.style.opacity='0'">
                <div style="flex: 1; text-align: left; font-weight: 600; color: #1e293b; padding-left: 8px; font-size: 14px;">${mecz.away}</div>
            `;
            listaMeczow.appendChild(row);
        });

        kontener.appendChild(listaMeczow);
    });
}

// Automatyczne uruchamianie obu lig na starcie strony
document.addEventListener("DOMContentLoaded", () => {
    if (typeof zmienKolejkeEkstraklasa === "function") zmienKolejkeEkstraklasa();
    zmienKolejkeLaLiga();
});
</script>



    <!-- IDENTYCZNA TABELA DLA LA LIGA -->
<div class="liga-section" style="margin-top: 30px;">
    <h2><span></span></h2>

    <!-- Wybór kolejki -->
    <div class="header-row" style="display: flex; align-items: center; justify-content: space-between; border-bottom: 2px solid #cbd5e1; padding-bottom: 10px; margin-bottom: 15px; font-family: 'Segoe UI', Helvetica, sans-serif;">
        <h2 style="margin: 0; font-size: 18px; color: #7a1c1c; font-weight: bold;">Wybierz kolejkę:</h2>
        
        <select id="kolejkaLaLigaSelect" onchange="zmienKolejkeLaLiga()" style="padding: 6px 12px; border: 1px solid #cbd5e1; border-radius: 4px; font-size: 14px; font-weight: bold; cursor: pointer; outline: none; background-color: #fff;">
            <option value="1">1. Kolejka</option>
            <option value="2">2. Kolejka</option>
            <option value="3">3. Kolejka</option>
            <option value="4">4. Kolejka</option>
            <option value="5">5. Kolejka</option>
            <option value="6">6. Kolejka</option>
            <option value="7">7. Kolejka</option>
            <option value="8">8. Kolejka</option>
            <option value="9">9. Kolejka</option>
            <option value="10">10. Kolejka</option>
            <option value="11" selected>11. Kolejka (Aktualna)</option>
            <option value="12">12. Kolejka</option>
            <option value="13">13. Kolejka</option>
        </select>
    </div>

    <!-- Tutaj skrypt JS automatycznie wygeneruje mecze -->
    <div id="kontenerMeczowLaLiga" style="font-family: 'Segoe UI', Helvetica, sans-serif; margin-bottom: 25px;"></div>

    <!-- Twoja tabela ligowa (zostaje bez zmian) -->
    <table class="ekstraklasa-table">


        <table class="ekstraklasa-table">
            <thead>
                <tr>
                    <th style="width: 60px;">Poz</th>
                    <th class="left-align">Klub</th>
                    <th style="width: 80px;">Mecze</th>
                    <th style="width: 100px;">Bramki</th>
                    <th style="width: 80px;">Pkt</th>
                </tr>
            </thead>
            <tbody>
                                    <!-- 1-4 Liga Mistrzów, 5-6 Liga Europy (Niebieskie strefy) -->
                    <tr class="zone-blue">
                        <td class="pos-num">1</td>
                        <td class="left-align team-name"><img src="herby/alaves.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Alaves</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-blue">
                        <td class="pos-num">2</td>
                        <td class="left-align team-name"><img src="herby/athletic-bilbao.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Athletic Bilbao</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-blue">
                        <td class="pos-num">3</td>
                        <td class="left-align team-name"><img src="herby/atletico-madryt.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Atletico Madryt</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-blue">
                        <td class="pos-num">4</td>
                        <td class="left-align team-name"><img src="herby/celta-vigo.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Celta Vigo</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-blue">
                        <td class="pos-num">5</td>
                        <td class="left-align team-name"><img src="herby/deportivo-a-coruna.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Deportivo A Coruña</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-blue">
                        <td class="pos-num">6</td>
                        <td class="left-align team-name"><img src="herby/elche-cf.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Elche CF</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    
                    <!-- Środek tabeli -->
                    <tr>
                        <td class="pos-num" style="color: #666;">7</td>
                        <td class="left-align team-name"><img src="herby/espanyol.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Espanyol</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">8</td>
                        <td class="left-align team-name"><img src="herby/fc-barcelona.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">FC Barcelona</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">9</td>
                        <td class="left-align team-name"><img src="herby/getafe-cf.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Getafe CF</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">10</td>
                        <td class="left-align team-name"><img src="herby/levante-ud.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Levante UD</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">11</td>
                        <td class="left-align team-name"><img src="herby/malaga-cf.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Málaga CF</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">12</td>
                        <td class="left-align team-name"><img src="herby/osasuna.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Osasuna</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">13</td>
                        <td class="left-align team-name"><img src="herby/racing-santander.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Racing Santander</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">14</td>
                        <td class="left-align team-name"><img src="herby/rayo-vallecano.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Rayo Vallecano</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">15</td>
                        <td class="left-align team-name"><img src="herby/real-betis.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Real Betis</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">16</td>
                        <td class="left-align team-name"><img src="herby/real-madryt.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Real Madryt</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr>
                        <td class="pos-num" style="color: #666;">17</td>
                        <td class="left-align team-name"><img src="herby/real-sociedad.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Real Sociedad</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>

                    <!-- 18-20 Strefa spadkowa (Czerwone strefy) -->
                    <tr class="zone-relegation">
                        <td class="pos-num">18</td>
                        <td class="left-align team-name"><img src="herby/sevilla-fc.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Sevilla FC</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-relegation">
                        <td class="pos-num">19</td>
                        <td class="left-align team-name"><img src="herby/valencia-cf.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Valencia CF</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>
                    <tr class="zone-relegation">
                        <td class="pos-num">20</td>
                        <td class="left-align team-name"><img src="herby/villarreal.png" alt="" style="height: 20px; width: auto; vertical-align: middle; margin-right: 8px;">Villarreal</td>
                        <td>0</td><td>0:0</td><td class="pts-cell">0</td>
                    </tr>

            </tbody>
        </table>
        <div class="legend">
            <div class="legend-item"><div class="dot dot-blue"></div> Strefa pucharowa</div>
            <div class="legend-item"><div class="dot dot-red"></div> Strefa spadkowa</div>
        </div>
    </div>
</main>
        <!-- BLOK DLA PREMIER LEAGUE -->
        <div id="widok-premier" class="liga-section" style="display: none; text-align: center; padding: 40px 20px;">
            <h2 style="justify-content: center; border-bottom: none;"><span>UK</span> Premier League</h2>
            <p style="font-size: 1.3em; color: #666; font-weight: 500; margin-top: 15px;">Wkrótce...</p>
        </div>


<!-- SKRYPT UKRYWAJĄCY I PRZEŁĄCZAJĄCY TABELE -->
<script>
    // 1. Łapiemy wszystkie trzy sekcje ligowe ze strony
    const sekcje = document.querySelectorAll('.liga-section');

    // Chowany La Ligę (indeks 1) oraz Premier League (indeks 2) na samym starcie
    if (sekcje.length >= 3) {
        sekcje[1].style.display = 'none'; // Ukryj La Liga
        sekcje[2].style.display = 'none'; // Ukryj Premier League (Wkrótce)
    }

    // 2. Bezbłędna funkcja zmieniająca widoki tabel i logotypy lig na górnym ciemnym pasku
    function przelaczWidok(kodLigi) {
        const logoGlowne = document.getElementById('league-logo');

        if (sekcje.length >= 3) {
            if (kodLigi === 'pl') {
                sekcje[0].style.display = 'block'; // Pokaż Ekstraklasę
                sekcje[1].style.display = 'none';  // Ukryj La Ligę
                sekcje[2].style.display = 'none';  // Ukryj Premier League
                if (logoGlowne) logoGlowne.src = 'herby/logo-ekstraklasa.png'; // Logo PL u góry
            } else if (kodLigi === 'es') {
                sekcje[0].style.display = 'none';  // Ukryj Ekstraklasę
                sekcje[1].style.display = 'block'; // Pokaż La Ligę
                sekcje[2].style.display = 'none';  // Ukryj Premier League
                if (logoGlowne) logoGlowne.src = 'herby/logo-laliga.png';     // Logo ES u góry
            } else if (kodLigi === 'uk') {
                sekcje[0].style.display = 'none';  // Ukryj Ekstraklasę
                sekcje[1].style.display = 'none';  // Ukryj La Ligę
                sekcje[2].style.display = 'block'; // Pokaż Premier League (Wkrótce)
                if (logoGlowne) logoGlowne.src = 'herby/logo-premier.png';    // Logo UK u góry
            }
        }
    }

    // 3. Obsługa kliknięć w przyciski Twoich zakładek
    const przyciski = document.querySelectorAll('.league-btn');
    przyciski.forEach(btn => {
        btn.removeAttribute('onclick'); // Czyszczenie starych powiązań inline
        
        btn.addEventListener('click', function() {
            // Przełączanie aktywnego podświetlenia zakładki
            przyciski.forEach(b => b.classList.remove('active'));
            this.classList.add('active');
            
            // Odczytujemy kliknięcie na podstawie tekstu na przycisku
            const tekst = this.innerText.toLowerCase();

            if (tekst.includes('ekstraklasa')) {
                document.getElementById('main-title').innerHTML = `<span>PL</span> PKO BP Ekstraklasa`;
                przelaczWidok('pl');
            } else if (tekst.includes('la liga')) {
                document.getElementById('main-title').innerHTML = `<span>ES</span> La Liga`;
                przelaczWidok('es');
            } else if (tekst.includes('premier')) {
                document.getElementById('main-title').innerHTML = `<span>UK</span> Premier League`;
                przelaczWidok('uk');
            }
        });
    });
</script>





</body>
</html>






