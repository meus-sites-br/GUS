<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Para Você</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">

    <style>
        /* --- Estilos do Cenário e Cartão --- */
        html,
        body {
          overflow: hidden;
          width: 100%;
          height: 100%;
          font-family: 'Roboto', sans-serif;
          position: relative; /* Necessário para as partículas */
        }

        body {
          display: flex;
          margin: auto;
          /* Centraliza o .scaler */
          justify-content: center;
          align-items: center;
          background-image: url(https://assets.codepen.io/4927073/Group2.png), linear-gradient(33deg, #2c303a, #2c303a);
          background-size: cover;
          background-position: 50% 50%;
        }

        /* NOVO: Container para escalar a animação */
        .scaler {
            transform: scale(1);
            transition: transform 0.3s ease-out;
            position: relative; /* Para o envelope se posicionar */
            width: 450px; /* Largura base para o envelope */
            height: 400px; /* Altura base */
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .envelope {
          content: url(https://assets.codepen.io/4927073/Envelope3.png);
          width: 430px;
          position: absolute;
          /* Posicionamento ajustado para o scaler */
          left: 10px;
          top: -20px;
          filter: drop-shadow(1.5px 0.75px 1.75px #4d4d4d);
          z-index: 1; 
        }

        .card {
          position: relative;
          width: 262px;
          height: 372px;
          /* margin: auto e left: 12px removidos, pois o scaler e o flex body já centram */
          box-shadow: inset 5px 0px 15px 0px rgba(0, 0, 0, 0.1),
            3px 0px 3px -2px rgba(0, 0, 0, 0.3);
          background-color: #fffffa;
          z-index: 2;
          
          /* EFEITO NOVO: Sombra pulsante */
          animation: cardShadowPulse 2.5s infinite alternate ease-in-out;
        }

        /* Keyframe para a sombra pulsante */
        @keyframes cardShadowPulse {
            0% {
                transform: scale(1.05);
                box-shadow: inset 5px 0px 15px 0px rgba(0, 0, 0, 0.1),
                            3px 0px 3px -2px rgba(0, 0, 0, 0.3);
            }
            100% {
                transform: scale(1.05); /* Mantém o scale original */
                box-shadow: inset 5px 0px 18px 0px rgba(0, 0, 0, 0.15),
                            5px 2px 8px -1px rgba(0, 0, 0, 0.4);
            }
        }


        .front {
          position: absolute;
          width: 100%;
          height: 100%;
          margin: auto;
          border: 1px solid #e0e0db;
          backface-visibility: hidden;
          
          /* CAPA NOVA: Gradiente de papel */
          background: linear-gradient(135deg, #f0efeb, #e0e0db);
          
          transform-style: preserve-3d;
          transform-origin: 0% 50%;
          transform: perspective(800px) rotateY(0deg);
          transition: all 1s ease-in-out;
          filter: saturate(1.125) brightness(1.0125);
          z-index: 5;
        }
        
        /* CAPA NOVA: Lacre de cera (sem imagem) */
        .front::after {
            content: '';
            position: absolute;
            width: 45px;
            height: 45px;
            background: #b71c1c; /* Vermelho escuro */
            border-radius: 50%;
            bottom: 25px;
            left: 50%;
            transform: translateX(-50%);
            /* Sombra realista para o lacre */
            box-shadow: 0 3px 6px rgba(0,0,0,0.3), 
                        inset 0 1px 2px rgba(255,255,255,0.4), 
                        inset 0 -2px 3px rgba(0,0,0,0.3);
            border: 2px solid #8c1010; /* Borda mais escura */
            opacity: 0.9;
        }


        .card:hover .front {
          transform: perspective(800px) rotateY(-170deg);
          background-color: rgb(205, 205, 205);
          /* EFEITO NOVO: Sombra da borda ao abrir */
          box-shadow: -2px 0px 5px rgba(0,0,0,0.1);
        }

        .card:hover .back {
          transform: perspective(800px) rotateY(-170deg);
          box-shadow: 7px 0px 2px 0px rgba(0, 0, 0, 0.3),
            inset 2px 0px 15px 0px rgba(0, 0, 0, 0.1);
          background-color: #fffffa;
        }

        .back {
          position: absolute;
          width: 100%;
          height: 100%;
          margin: auto;
          backface-visibility: visible;
          filter: drop-shadow(2px 2px 4px rgba(0,0,0,0.25));
          box-shadow: inset 5px 0px 15px 0px rgba(0, 0, 0, 0.1),
            3px 0px 3px 1px rgba(0, 0, 0, 0.3);
          transform-style: preserve-3d;
          transform-origin: 0% 50%;
          transform: perspective(800px) rotateY(0deg);
          transition: all 1s ease-in-out;
          background-color: #fff;
          z-index: 4;
        }

        .text-container {
          width: 80%;
          height: 80%;
          margin: auto;
          display: flex;
          flex-direction: column; 
          justify-content: center;
          align-items: center;
          color: #333;
          z-index: 3;
        }

        .f-box {
          width: 100%;
          height: 100%;
          margin: auto;
          background-color: transparent;
          box-shadow: 0 2px rgba(0, 0, 0, 0.1);
          text-align: center;
        }

        /* .front-text removido */

        .inside-text {
            font-size: 1.1em;
            text-align: center;
            margin-bottom: 15px;
            color: #444;
        }


        /* --- Estilos do Batimento Cardíaco --- */
        svg {
          overflow: visible;
          width: 100%;
          max-width: 400px;
        }

        svg path#line {
          fill: none;
          stroke: #e00000;
          stroke-width: 2;
          stroke-linecap: butt;
          stroke-linejoin: round;
          stroke-miterlimit: 4;
          stroke-dasharray:none;
          stroke-opacity: 1;
          stroke-dasharray: 1;
          stroke-dashoffset: 1;
          animation: dash 4s linear infinite; 
        }

        svg path#heart {
          transform-origin: 50% 50%;
          animation: blink 4s linear infinite;
        }


        /* --- Animações (Keyframes) --- */
        @keyframes dash {
          0% {
            stroke-dashoffset: 1;
          }
          80% {
            stroke-dashoffset: 0;
          }
          100% {
            stroke-dashoffset: 0;
          }
        }

        @keyframes blink {
          0% {
            opacity: 0;
            transform: scale(0);
          }
          60% {
            opacity: 0;
            transform: scale(0);
          }
          70% {
            opacity: 1;
            transform: scale(1.2);
          }
          75% {
            opacity: 1;
            transform: scale(1.0);
          }
          80% {
            opacity: 1;
            transform: scale(1.2);
          }
          85% {
            opacity: 1;
            transform: scale(1.0);
          }
          100% {
            opacity: 0;
            transform: scale(1.0);
          }
        } 


        /* --- EFEITO NOVO: Partículas de Coração Flutuantes --- */
        .heart-particle {
            position: absolute;
            background-color: #ff6666; /* Cor do coração */
            border-radius: 50%;
            opacity: 0;
            animation: floatHeart 10s infinite ease-in-out;
            pointer-events: none; /* Para não interferir com o mouse */
            z-index: 0; /* Atrás de tudo */
        }
        .heart-particle:nth-child(3n+1) { width: 10px; height: 10px; }
        .heart-particle:nth-child(3n+2) { width: 15px; height: 15px; }
        .heart-particle:nth-child(3n+3) { width: 8px; height: 8px; }
        .heart-particle:nth-child(1) { left: 10%; top: 80%; animation-delay: 0s; }
        .heart-particle:nth-child(2) { left: 20%; top: 90%; animation-delay: 1s; }
        .heart-particle:nth-child(3) { left: 30%; top: 70%; animation-delay: 2s; }
        .heart-particle:nth-child(4) { left: 40%; top: 85%; animation-delay: 3s; }
        .heart-particle:nth-child(5) { left: 50%; top: 75%; animation-delay: 4s; }
        .heart-particle:nth-child(6) { left: 60%; top: 80%; animation-delay: 5s; }
        .heart-particle:nth-child(7) { left: 70%; top: 90%; animation-delay: 6s; }
        .heart-particle:nth-child(8) { left: 80%; top: 65%; animation-delay: 7s; }
        .heart-particle:nth-child(9) { left: 90%; top: 88%; animation-delay: 8s; }
        .heart-particle:nth-child(10) { left: 5%; top: 72%; animation-delay: 9s; }

        @keyframes floatHeart {
            0% {
                transform: translateY(0) rotate(0deg) scale(0);
                opacity: 0;
            }
            10% {
                opacity: 0.8;
                transform: translateY(-50px) rotate(20deg) scale(0.8);
            }
            80% {
                opacity: 0.2;
                transform: translateY(-600px) rotate(-20deg) scale(1.2);
            }
            100% {
                transform: translateY(-700px) rotate(0deg) scale(0);
                opacity: 0;
            }
        }

        /* --- Media Queries (Responsividade com Scale) --- */
        
        /* Telas médias (tablets, paisagem) */
        @media (max-width: 768px), (max-height: 600px) {
          .scaler {
            transform: scale(0.8);
          }
        }

        /* Telas pequenas (celulares) */
        @media (max-width: 480px), (max-height: 480px) {
          .scaler {
            transform: scale(0.6);
          }
        }
        
        /* Telas muito pequenas */
        @media (max-width: 360px) {
            .scaler {
                transform: scale(0.5);
            }
        }
    </style>
</head>
<body>

    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>
    <div class="heart-particle"></div>


    <div class="scaler">
        <div class="envelope"></div>

        <div class="card">
            <div class="back"></div>
            <div class="front">
                <div class="f-box">
                    </div>
            </div>

            <div class="text-container">
                <p class="inside-text">Mesmo de longe, você me faz bem.</p>
                
                <svg xmlns="http://www.w3.org/2000/svg" id="svg5" version="1.1" viewBox="0 0 502.98 108.61">
                  <path id="heart" d="M213.35 29.43c19.41-15.19 33.68 10.86 37.73 18.82-.28-13.61 11.64-40.98 25.94-34.01 32.3 15.74-15.88 83.8-26.4 81.76-13.24-9-51.35-53.3-37.27-66.57Z" style="fill:#ff9999;stroke:#ff9999;stroke-width:1.5;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1"/>
                  <path pathLength="1" id="line" d="M5.32 78.13c.96-.01 5-8.5 5.54-8.54.58-.05 6.1 8.51 7.1 8.51 3.66 0 9.29.06 10.71.05 2.53-.01 4.82-72.88 4.82-72.88l4.76 97.28 3.97-24.45 20.45-.22C64 77.86 77.1 63.66 78.36 63.8c1.31.15 6.68 14.08 7.94 14.07 2.3-.03 33.32.04 35.76.02.96 0 5-8.5 5.53-8.53.59-.05 6.1 8.51 7.1 8.5 3.66 0 9.3.07 10.72.06 2.53-.02 4.81-72.89 4.81-72.89l4.77 97.28 3.97-24.44s83.34-3.33 74.69 7.67c-8.65 11-45.3-42.94-31.65-53.58 25.6-19.96 49.96 36.94 40.26 36.5-12.2-.53 1.8-62.32 23.41-51.7 32.24 15.86-17.56 84.92-26.4 81.77-5.73-2.05-.68-21.68 31.4-26.58 26.65-6.42 29.5 2.35 52.62 7.11 2.53-.02 4.82-72.89 4.82-72.89l4.76 97.28 3.97-24.44 20.45-.22c1.31-.02 14.41-14.22 15.68-14.07 1.32.15 6.7 14.08 7.95 14.07 2.29-.03 33.32.04 35.76.02.95 0 5-8.5 5.53-8.54.58-.04 6.1 8.52 7.1 8.52 3.66 0 9.3.06 10.72.05 2.53-.02 4.81-72.89 4.81-72.89l4.77 97.28 3.97-24.44 20.45-.23c1.31-.01 14.4-14.22 15.68-14.07 1.32.16 6.69 14.09 7.94 14.07" />
                </svg>
            </div>
        </div>
    </div>

</body>
</html>
