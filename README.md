# nslookup
nslookup
#!/usr/bin/env python3
"""
nslookup_tool.py
Program sederhana untuk melakukan NS Lookup berbagai record DNS.

Contoh penggunaan:
  python nslookup_tool.py google.com
  python nslookup_tool.py example.com --type MX
  python nslookup_tool.py openai.com --type NS
"""

import argparse
import dns.resolver

def nslookup(domain, record_type="A"):
    """
    Melakukan query DNS untuk domain dan tipe tertentu.
    """
    try:
        answers = dns.resolver.resolve(domain, record_type)
        print(f"\nHasil NS Lookup untuk {domain} (record {record_type}):")
        print("-" * 60)
        for rdata in answers:
            print(rdata.to_text())
    except dns.resolver.NoAnswer:
        print(f"Tidak ada jawaban untuk {record_type} record.")
    except dns.resolver.NXDOMAIN:
        print(f"Domain {domain} tidak ditemukan.")
    except Exception as e:
        print(f"Terjadi kesalahan: {e}")

def main():
    parser = argparse.ArgumentParser(description="Program sederhana untuk NS Lookup berbagai record DNS.")
    parser.add_argument("domain", help="Nama domain yang ingin dicari (misal: google.com)")
    parser.add_argument("--type", default="A", help="Tipe record DNS (A, AAAA, MX, NS, TXT, CNAME, SOA, dsb.)")
    args = parser.parse_args()

    nslookup(args.domain, args.type.upper())

if __name__ == "__main__":
    main()
