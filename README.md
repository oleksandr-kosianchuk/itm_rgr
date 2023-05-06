# Мета розрахункової роботи

Автоматизувати створення віртуальної машини за допомогою Terraform та CI/CD. В межах цієї роботи для прикладу було обрано створення та хостинг вебсайту.

# Хід роботи

Створення віртуальної машини, та автоматизація цього процесу за допомогою Terraform вже було виконано у попередніх лабораторних роботах, тому я скористуюся цією віртульною машиной, але треба внести деякі правки до файлів конфігураціЇ, а саме:
```
resource "google_compute_firewall" "firewall_rules" {
  project       = var.project
  name          = "test-firewall-rule"
  network       = google_compute_network.vpc_network.name
  description   = "Enables all instances in the vpc_network to be able to be connected to from external devices"
  source_ranges = ["0.0.0.0/0"]

  allow {
    protocol = "tcp"
    ports = ["22", "8080"]
  }
}
```
Я додав це до файлу main.tf у кінці. Це правило для файрвола, яке відкриває додаткові порти. Це необхідно для ssh підключення до нашого сервера.

Далі, виконуємо команду terraform apply у powerShell локально та дивимось, чи спрацювали налатуштування для файрвола.

![](/images/ssh_1.png)




Все добре ! Як бачимо, тепер ми можемо керувати нашою віртуальною машину за допомогою ssh з'єднання.



Тепер встановимо необхідні пакети для роботи. Спочатку оновимо репозиторії.

![](/images/ssh_update.png)




Далі, встановлюємо git.

![](/images/ssh_git_install.png)




Також, java та sdk для подальшої роботи jenkins. 

![](/images/ssh_open_jdk_install.png)




Тепер, встановими безпосередньо Jenkins.

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins
```
