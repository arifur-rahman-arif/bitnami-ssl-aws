# SSL Installation & Management for Bitnami on AWS

This guide explains how to **install, reinstall, and manage SSL certificates** for a Bitnami WordPress instance hosted on AWS using **Let’s Encrypt** via the **Bitnami BNCert Tool**.

## Prerequisites

- An AWS EC2 instance running **Bitnami WordPress**.
- A registered domain pointing to your instance’s public IP.
- SSH access with your **private key (`.pem`)**.

---

## 1. Connect to the Server

Use SSH to connect:

```bash
ssh -i /path/to/your-key.pem bitnami@YOUR_EC2_PUBLIC_IP
````

Replace:

* `/path/to/your-key.pem` → your SSH private key.
* `YOUR_EC2_PUBLIC_IP` → your EC2 instance IP.

---

## 2. Backup Existing Certificates (Optional)

Before reinstalling SSL:

```bash
sudo cp -r /opt/bitnami/letsencrypt /opt/bitnami/letsencrypt-backup
```

---

## 3. Install or Check Bitnami BNCert Tool

### Check if BNCert Tool is installed:

```bash
sudo /opt/bitnami/bncert-tool
```

### If not installed, download and set permissions:

```bash
cd /tmp
curl -LO https://downloads.bitnami.com/files/bncert/latest/bncert-linux-x64.run
chmod +x bncert-linux-x64.run
sudo mv bncert-linux-x64.run /opt/bitnami/bncert-tool
```

---

## 4. Run the BNCert Tool

```bash
sudo /opt/bitnami/bncert-tool
```

The tool will prompt you for:

1. **Domain name(s)** (e.g., `example.com,www.example.com`)
2. **Email address** for Let’s Encrypt notifications
3. Enable **HTTP → HTTPS redirection** (`Y` recommended)
4. Enable **automatic certificate renewal** (`Y` recommended)

The tool will:

* Request SSL certificates from Let’s Encrypt
* Configure Apache with SSL
* Enable HTTP → HTTPS redirection if chosen
* Set up automatic renewal

---

## 5. Restart Apache (if needed)

```bash
sudo /opt/bitnami/ctlscript.sh restart apache
```

---

## 6. Reinstall or Renew SSL

To reinstall or renew an SSL certificate, simply run:

```bash
sudo /opt/bitnami/bncert-tool
```

Follow the prompts again to update or renew your certificate.

---

## 7. Verify SSL

Open your site in a browser:

```
https://yourdomain.com
```

You should see a valid SSL padlock.

---

## Notes

* Ensure your **domain DNS points to your EC2 instance**.
* Certificates **expire every 90 days**; enable automatic renewal.
* Always **backup existing certificates** before reinstalling.

---

## References

* [Bitnami SSL Guide](https://docs.bitnami.com/aws/how-to/understand-bncert/)
* [Let’s Encrypt Documentation](https://letsencrypt.org/docs/)

