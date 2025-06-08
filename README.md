# PRAKTIKUM 4-6

![CodeIgniter Badge](https://img.shields.io/badge/CodeIgniter-%23EF4223.svg?style=for-the-badge&logo=codeIgniter&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)

## Profil
| Nama | Kelas | NIM | Mata Kuliah | Dosen |
|------|-------|-----|-------------|-------|
| LUTPIAH AINUS SHIDDIK    |  TI.23.A.5     | 312310474    |    Pemograman web 2         |  Agung Nugroho, S.Kom., M.Kom.     |

# Praktikum 4: Framework Lanjutan (Modul Login)

# Langkah-langkah Praktikum

Untuk memulai membuat modul Login, yang perlu disiapkan adalah database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

# 1. Persiapkan Database
Buat tabel user pada database dengan SQL berikut:

```
CREATE TABLE user (
  id INT(11) auto_increment,
  username VARCHAR(200) NOT NULL,
  useremail VARCHAR(200),
  userpassword VARCHAR(200),
  PRIMARY KEY(id)
);
```
![database](https://github.com/user-attachments/assets/4f3a4de4-d21c-4a2b-ab89-842afb684736)

# 2. Membuat Model User
Selanjutnya adalah membuat Model untuk memproses data Login. Buat file baru pada direktori app/Models dengan nama UserModel.php

```php
<?php
namespace App\Models;
use CodeIgniter\Model;

class UserModel extends Model
{
    protected $table = 'user';
    protected $primaryKey = 'id';
    protected $useAutoIncrement = true;
    protected $allowedFields = ['username', 'useremail', 'userpassword'];
}
```

# 3.Membuat Controller User
Buat Controller baru dengan nama User.php pada direktori app/Controllers.

Kemudian tambahkan method index() untuk menampilkan daftar user, dan method login() untuk proses login.

```php
<?php

namespace App\Controllers;

use App\Models\UserModel;

class User extends BaseController
{
    public function index()
    {
        $title = 'Daftar User';
        $model = new UserModel();
        $users = $model->findAll();
        return view('user/index', compact('users', 'title'));
    }
    public function login()
    {
        helper(['form']);
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');
        if (!$email)
        {
        return view('user/login');
        }

        $session = session();
        $model = new UserModel();
        $login = $model->where('useremail', $email)->first();
        if ($login)
        {
            $pass = $login['userpassword'];
            if (password_verify($password, $pass))
            {
                $login_data = [
                'user_id' => $login['id'],
                'user_name' => $login['username'],
                'user_email' => $login['useremail'],
                'logged_in' => TRUE,
                ];

                $session->set($login_data);
                return redirect('admin/artikel');
            }
            else
            {
                $session->setFlashdata("flash_msg", "Password salah.");
                return redirect()->to('/user/login');
            }
        }
        else
        {
            $session->setFlashdata("flash_msg", "email tidak terdaftar.");
            return redirect()->to('/user/login');
        }
    }
}
```

# 4. Membuat View Login
Buat direktori baru dengan nama user pada direktori app/views, kemudian buat file baru dengan nama login.php.

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<title>Login</title>
		<link rel="stylesheet" href="<?= base_url('/style.css'); ?>" />
	</head>

	<body>
		<div id="login-wrapper">
			<h1>Sign In</h1>
			<?php if (session()->getFlashdata('flash_msg')): ?>
			<div class="alert alert-danger">
				<?= session()->getFlashdata('flash_msg') ?>
			</div>
			<?php endif; ?>
			<form action="" method="post">
				<div class="mb-3">
					<label for="InputForEmail" class="form-label">Email address</label>
					<input
						type="email"
						name="email"
						class="form-control"
						id="InputForEmail"
						value="<?= set_value('email') ?>"
					/>
				</div>
				<div class="mb-3">
					<label for="InputForPassword" class="form-label">Password</label>

					<input
						type="password"
						name="password"
						class="form-control"
						id="InputForPassword"
					/>
				</div>
				<button type="submit" class="btn btn-primary">Login</button>
			</form>
		</div>
	</body>
</html>
```
