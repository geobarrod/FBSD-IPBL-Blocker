# FBSD-IPBL-Blocker
Downloads multiple external IP blocklists, merges them, sorts them, and loads them into persistent PF tables for automatic blocking of malicious sources.   It dynamically adjusts `net.pf.request_maxcount` based on the amount of IPs inside of blocklist file.
