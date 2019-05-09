# Đặt tên cho backend, hãy nhớ chỉnh luôn ở global.conf
backend trangweb1 {
        .host = "127.0.0.1";
        .port = "8181";
}

sub vcl_recv {
        # Nhớ đổi domain
        if (req.http.host ~ "domain1cuaban.com") {
           set req.backend_hint = trangweb1;
        }

        # Cache các file sau đây
        if (req.url ~ "(?i)\.(png|gif|jpeg|jpg|ico|swf|css|js|html|htm)(\?[a-z0-9]+)?$") {
                unset req.http.Cookie;
        }

        # Xóa cookie Analytic
        set req.http.Cookie = regsuball(req.http.Cookie, "has_js=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "__utm.=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "_ga=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "utmctr=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "utmcmd.=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "utmccn.=[^;]+(; )?", "");

        # Không cache Ajax
        if (req.http.X-Requested-With == "XMLHttpRequest") {
                return(pass);
        }

        # Không cache POST
        if (req.http.Authorization || req.method == "POST") {
                return (pass);
        }
        if (req.method != "GET" && req.method != "HEAD") {
                return (pass);
        }

        # Không cache truy vấn xác thực
        if (req.http.Authorization) {
                return(pass);
        }

        # Passthrough cho LetsEncrypt Certbot
        if (req.url ~ "^/\.well-known/acme-challenge/") {
                return (pass);
        }

        # Chuyển tiếp IP của khách cho Backend, tránh tình trang toàn IP của Server (Nginx)
        if (req.restarts == 0) {
                if (req.http.X-Real-IP) {
                        set req.http.X-Forwarded-For = req.http.X-Real-IP;
                } else if (req.http.X-Forwarded-For) {
                        set req.http.X-Forwarded-For = req.http.X-Forwarded-For + ", " + client.ip;
                } else {
                        set req.http.X-Forwarded-For = client.ip;
                }
        }

        ### Wordpress ###
        if (req.url ~ "(wp-admin|post\.php|edit\.php|wp-login)") {
                return(pass);
        }
        if (req.url ~ "/wp-cron.php" || req.url ~ "preview=true") {
                return (pass);
        }

        # WP-Affiliate
        if ( req.url ~ "\?ref=" ) {
                return (pass);
        }

        set req.http.Cookie = regsuball(req.http.Cookie, "wp-settings-1=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "wp-settings-time-1=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "wordpress_test_cookie=[^;]+(; )?", "");
        set req.http.Cookie = regsuball(req.http.Cookie, "PHPSESSID=[^;]+(; )?", "");

        return (hash);
}
