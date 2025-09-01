# ---- SPACE FIRST AID (safe cleanup) ----
set -e

echo "[1/7] Show current free space"
df -h /

echo "[2/7] Remove large, non-essential caches & temp"
sudo apt-get clean -y || true
sudo rm -rf /var/lib/apt/lists/* || true
sudo rm -rf /var/cache/apt/archives/* || true
sudo journalctl --vacuum-time=7d || true
sudo rm -rf /tmp/* /var/tmp/* || true
rm -rf ~/.cache/pip ~/.cache/thumbnails || true

echo "[3/7] Remove common big leftovers if present"
rm -rf ~/Downloads/* 2>/dev/null || true
rm -rf ~/Videos/* 2>/dev/null || true
rm -rf ~/pytorch 2>/dev/null || true   # incomplete clone often huge
docker system prune -af --volumes 2>/dev/null || true

echo "[4/7] Recreate temp dirs with correct perms"
sudo mkdir -p /tmp /var/tmp /usr/tmp
sudo chown root:root /tmp /var/tmp /usr/tmp
sudo chmod 1777 /tmp /var/tmp /usr/tmp

echo "[5/7] (Optional) mount /tmp as RAM tmpfs to avoid disk usage"
# Uses RAM; change size if you prefer
sudo mount -t tmpfs -o rw,nosuid,nodev,mode=1777,size=512M tmpfs /tmp || true

echo "[6/7] Verif
