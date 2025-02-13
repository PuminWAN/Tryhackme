# Enumeration & Brute Force

---

### Introduction
Introduction
Authentication enumeration is a fundamental aspect of security testing, concentrating specifically on the mechanisms that protect sensitive aspects of web applications; this process involves methodically inspecting various authentication components ranging from username validation to password policies and session management. Each of these elements is meticulously tested because they represent potential vulnerabilities that, if exploited, could lead to significant security breaches.

Objectives
By the end of this room, you will:

Understand the significance of enumeration and how it sets the stage for effective brute-force attacks.
Learn advanced enumeration methods, mainly focusing on extracting information from verbose error messages.
Comprehend the relationship between enumeration and brute-force attacks in compromising authentication mechanisms.
Gain practical experience using tools and techniques for both enumeration and brute-force attacks.
Pre-requisites
Before starting this room, you should have a basic understanding of the following concepts:

Familiarity with HTTP and HTTPS, including request/response structures and common status codes.
Experience using tools like Burp Suite.
Basic proficiency in navigating and using the Linux command line.
Answer the questions below
Deploy the target VM attached to this task by pressing the green Start Machine button. After obtaining the machine's generated IP address, you can either use the AttackBox or your own VM connected to TryHackMe's VPN.

Add 10.10.110.132 to your /etc/hosts file. For example:

/etc/hosts
10.10.110.132    enum.thm

After 3 minutes, visit http://enum.thm to access the machine. We recommend using the AttackBox for this room.


Authentication Enumeration
Fingerprint icon

Think of yourself as a digital detective. It's not just about picking up clues—it's about understanding what these clues reveal about the security of a system. This is essentially what authentication enumeration involves. It's like piecing together a puzzle rather than just ticking off items on a checklist.

Authentication enumeration is like peeling back the layers of an onion. You remove each layer of a system's security to reveal the real operations underneath. It's not just about routine checks; it's about seeing how everything is connected.

Identifying Valid Usernames

Knowing a valid username lets an attacker focus just on the password. You can figure out usernames in different ways, like observing how the application responds during login or password resets. For example, error messages that specify "this account doesn't exist" or "incorrect password" can hint at valid usernames, making an attacker's job easier.

Password Policies

The guidelines when creating passwords can provide valuable insights into the complexity of the passwords used in an application. By understanding these policies, an attacker can gauge the potential complexity of the passwords and tailor their strategy accordingly. For example, the below PHP code uses regex to require a password that includes symbols, numbers, and uppercase letters:

<?php
$password = $_POST['pass']; // Example1
$pattern = '/^(?=.*[A-Z])(?=.*\d)(?=.*[\W_]).+$/';

if (preg_match($pattern, $password)) {
    echo "Password is valid.";
} else {
    echo "Password is invalid. It must contain at least one uppercase letter, one number, and one symbol.";
}
?>
In the above example, if the supplied password doesn't satisfy the policy defined in the pattern variable, the application will return an error message revealing the regex code requirement. An attacker might generate a dictionary that satisfies this policy.

Common Places to Enumerate
Web applications are full of features that make things easier for users but can also expose them to risks:

Registration Pages

Web applications typically make the user registration process straightforward and informative by immediately indicating whether an email or username is available. While this feedback is designed to enhance user experience, it can inadvertently serve a dual purpose. If a registration attempt results in a message stating that a username or email is already taken, the application is unwittingly confirming its existence to anyone trying to register. Attackers exploit this feature by testing potential usernames or emails, thus compiling a list of active users without needing direct access to the underlying database.

Password Reset Features

Password reset mechanisms are designed to help users regain access to their accounts by entering their details to receive reset instructions. However, the differences in the application's response can unintentionally reveal sensitive information. For example, variations in an application's feedback about whether a username exists can help attackers verify user identities. By analyzing these responses, attackers can refine their lists of valid usernames, substantially improving the effectiveness of subsequent attacks.

Verbose Errors

Verbose error messages during login attempts or other interactive processes can reveal too much. When these messages differentiate between "username not found" and "incorrect password," they're intended to help users understand their login issues. However, they also provide attackers with definitive clues about valid usernames, which can be exploited for more targeted attacks.

Data Breach Information

Data from previous security breaches is a goldmine for attackers as it allows them to test whether compromised usernames and passwords are reused across different platforms. If an attacker finds a match, it suggests not only that the username is reused but also potential password recycling, especially if the platform has been breached before. This technique demonstrates how the effects of a single data breach can ripple through multiple platforms, exploiting the connections between various online identities.

Answer the questions below
What type of error messages can unintentionally provide attackers with confirmation of valid usernames?

