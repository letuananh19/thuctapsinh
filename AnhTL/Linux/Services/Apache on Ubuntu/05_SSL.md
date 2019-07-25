# Cấu hình SSL

B1: kích hoạt ssl.
```
a2enmod ssl
```

B2: kích hoạt file default-ssl

- Kích hoạt tệp cấu hình HTTPS mặc định trong ``/etc/apache2/sites-available/default-ssl.conf`` . Để Apache2 cung cấp HTTPS
```
a2ensite default-ssl
```

B3: restart lại dịch vụ
```
systemctl restart apache2.service
```

![](https://i.imgur.com/tlOgK04.png)
