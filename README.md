
socket di importazione
import argparse
import sys

# Emoji
globe = "🌍"
target = "🎯"
port = "🔌"
success = "✅"
fail = "❌"
dns = "🧠"

# Porte comuni
common_ports = {
    21: 'FTP',
    22: 'SSH',
    23: 'Telnet',
    25: 'SMTP',
    53: 'DNS',
    80: 'HTTP',
    110: 'POP3',
    143: 'IMAP',
    443: 'HTTPS',
    3306: 'MySQL',
    8080: 'HTTP-Alt'
}

def scan_ports(ip):
    print(f"{port} Scansione porte su {ip} in corso...\n")
    for p in common_ports:
        try:
            s = socket.socket()
            s.settimeout(0.5)
            s.connect((ip, p))
            print(f"{success} Porta {p} aperta ({common_ports[p]})")
            s.close()
        except:
            pass

def resolve_dns(domain):
    try:
        ip = socket.gethostbyname(domain)
        print(f"{dns} Risolto {domain} ➜ {ip}")
        return ip
    except socket.gaierror:
        print(f"{fail} Errore DNS per {domain}")
        sys.exit()

def main():
    parser = argparse.ArgumentParser(description="Tool di scansione IP, DNS e porte 🔍")
    parser.add_argument("-d", "--domain", help="Dominio da risolvere")
    parser.add_argument("-i", "--ip", help="IP da scansionare")

    args = parser.parse_args()

    ip = ""
    if args.domain:
        ip = resolve_dns(args.domain)
    elif args.ip:
        ip = args.ip
    else:
        print("❗ Specifica un dominio (-d) o un IP (-i)")
        sys.exit()

    print(f"{globe} Target IP: {ip}")
    scan_ports(ip)

if __name__ == "__main__":
    main()
