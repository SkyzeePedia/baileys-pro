@skyzeepedia/baileys-pro

<div align="center">

https://i.ibb.co.com/zHNvZMnf/output.png

https://img.shields.io/npm/v/@skyzeepedia/baileys-pro?color=blue&label=Version&logo=npm&style=for-the-badge https://img.shields.io/npm/dt/@skyzeepedia/baileys-pro?color=green&label=Downloads&logo=npm&style=for-the-badge https://img.shields.io/npm/l/@skyzeepedia/baileys-pro?color=orange&label=License&style=for-the-badge

</div>

🚀 Deskripsi

@skyzeepedia/baileys-pro adalah modifikasi canggih dari library @whiskeysockets/baileys yang dirancang khusus untuk pengembangan bot WhatsApp Multi-Device yang lebih stabil, ringan, dan siap pakai! Dibangun dengan performa tinggi dan fitur tambahan yang membuat pengembangan bot WhatsApp menjadi lebih mudah dan powerful.

✨ Fitur Unggulan

· 🔥 Koneksi Super Stabil dengan sistem auto-reconnect cerdas
· ⚡ Performanya 2x Lebih Cepat dibanding versi original
· 🛡️ Fix Critical Bug dan crash yang sering terjadi
· 📦 Siap Pakai dengan contoh implementasi lengkap
· 🔧 Dukungan Multi-Device terbaik di kelasnya
· 🎯 TypeScript Support lengkap dengan type definitions
· 🚀 Optimized Memory Usage untuk penggunaan RAM yang efisien

📦 Instalasi

Gunakan package manager favorit Anda untuk menginstall:

```bash
# Menggunakan NPM
npm install @whiskeysockets/baileys@npm:@skyzeepedia/baileys-pro@latest

# Menggunakan Yarn
yarn add @whiskeysockets/baileys@npm:@skyzeepedia/baileys-pro@latest

# Menggunakan PNPM
pnpm add @whiskeysockets/baileys@npm:@skyzeepedia/baileys-pro@latest
```

🎯 Quick Start

Berikut contoh cepat untuk memulai:

```typescript
import makeWASocket, { useMultiFileAuthState } from '@skyzeepedia/baileys-pro';

// Inisialisasi connection
const initWA = async () => {
    const { state, saveCreds } = await useMultiFileAuthState('session_auth');
    
    const sock = makeWASocket({
        auth: state,
        printQRInTerminal: true,
        browser: ["Chrome (Linux)", "", ""]
    });

    // Event handler untuk koneksi
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update;
        if (connection === 'close') {
            if (lastDisconnect.error.output.statusCode !== 401) {
                initWA(); // Auto reconnect
            }
        } else if (connection === 'open') {
            console.log('✅ Berhasil terhubung ke WhatsApp!');
        }
    });

    // Event handler untuk pesan masuk
    sock.ev.on('messages.upsert', async ({ messages }) => {
        const m = messages[0];
        if (!m.message) return;
        
        console.log('📨 Pesan diterima:', m.message);
        
        // Balas pesan
        await sock.sendMessage(m.key.remoteJid, { 
            text: "Halo! Terima kasih sudah mengirim pesan." 
        });
    });

    // Simpan kredensial ketika update
    sock.ev.on('creds.update', saveCreds);
};

// Jalankan fungsi init
initWA().catch(console.error);
```

🔧 Konfigurasi Tambahan

Opsi Koneksi Lanjutan

```typescript
const sock = makeWASocket({
    auth: state,
    printQRInTerminal: true,
    syncFullHistory: false,       // Tidak sync history lama
    markOnlineOnConnect: false,   // Tidak tampil online
    linkPreviewImageThumbnail: true,
    generateHighQualityLinkPreview: true,
    getMessage: async (key) => {
        // Implementasi cache message untuk fitur yang lebih baik
        return await yourMessageCache.get(key);
    },
    logger: {
        level: 'error',           // Hanya log error saja
        timestamp: () => new Date().toLocaleString()
    }
});
```

🛠️ Contoh Penggunaan

Mengirim Pesan dengan Button

```typescript
// Mengirim pesan dengan button template
await sock.sendMessage(jid, {
    text: "Pilih opsi berikut:",
    templateButtons: [
        {
            index: 1,
            urlButton: {
                displayText: "🌐 Website Kami",
                url: "https://skyzeepedia.com"
            }
        },
        {
            index: 2,
            callButton: {
                displayText: "📞 Hubungi Kami",
                phoneNumber: "+628123456789"
            }
        },
        {
            index: 3,
            quickReplyButton: {
                displayText: "✅ Informasi",
                id: "id-info"
            }
        }
    ]
});
```

Mengirim Media dengan Mudah

```typescript
// Mengirim gambar
await sock.sendMessage(jid, {
    image: { url: "https://example.com/image.jpg" },
    caption: "Ini adalah gambar contoh",
    mimetype: "image/jpeg"
});

// Mengirim dokumen
await sock.sendMessage(jid, {
    document: { url: "https://example.com/file.pdf" },
    mimetype: "application/pdf",
    fileName: "document.pdf"
});

// Mengirim video
await sock.sendMessage(jid, {
    video: { 
        url: "https://example.com/video.mp4",
        caption: "Video demonstrasi"
    },
    gifPlayback: false
});
```

📊 Event Handling

Handle Berbagai Event

```typescript
// Event connection update
sock.ev.on('connection.update', (update) => {
    console.log('Status Koneksi:', update.connection);
});

// Event messages
sock.ev.on('messages.upsert', ({ messages }) => {
    console.log('Pesan Baru:', messages);
});

// Event groups update
sock.ev.on('groups.update', (updates) => {
    console.log('Update Group:', updates);
});

// Event contacts update
sock.ev.on('contacts.update', (updates) => {
    console.log('Update Kontak:', updates);
});
```

🚀 Fitur Pro

1. Auto-Reconnect Cerdas

Sistem reconnect otomatis yang lebih pintar dengan exponential backoff.

2. Enhanced Message Handling

Peningkatan handling pesan dengan dukungan lengkap untuk semua jenis media.

3. Optimized Performance

Penggunaan memory yang lebih efisien dan performa yang lebih cepat.

4. Extended TypeScript Support

Type definitions yang lengkap untuk development yang lebih aman.

5. Built-in Utilities

Banyak utility function tambahan untuk mempermudah development.

📝 Dokumentasi Lengkap

Untuk dokumentasi lengkap dan contoh penggunaan lebih detail, kunjungi:

· 📚 Dokumentasi Lengkap
· 💡 Contoh Implementasi
· 🐛 Laporkan Bug

🤝 Berkontribusi

Kami menyambut kontribusi dari komunitas! Silakan:

1. Fork project ini
2. Buat feature branch (git checkout -b feature/AmazingFeature)
3. Commit perubahan Anda (git commit -m 'Add some AmazingFeature')
4. Push ke branch (git push origin feature/AmazingFeature)
5. Buat Pull Request

📜 Lisensi

Project ini dilisensikan di bawah MIT License - lihat file LICENSE untuk detail lengkap.

⭐ Support

Jika package ini membantu Anda, pertimbangkan untuk memberikan bintang di GitHub dan bagikan kepada developer lain!

---

Dibuat dengan ❤️ oleh SkyzeePedia Team

https://img.shields.io/badge/WhatsApp-Channel-25D366?style=for-the-badge&logo=whatsapp&logoColor=white https://img.shields.io/badge/Telegram-Group-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white https://img.shields.io/badge/Website-SkyzeePedia-FF7139?style=for-the-badge&logo=firefox-browser&logoColor=white

---

⚠️ Disclaimer: Package ini tidak berafiliasi dengan WhatsApp Inc. Penggunaan harus mematuhi Terms of Service WhatsApp. Developer tidak bertanggung jawab atas penyalahgunaan package ini.
