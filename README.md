# home-start
홈페이지 만들기
import React, { useEffect, useState } from "react";
import { motion } from "framer-motion";
import { Menu, X, ArrowRight, Moon, Sun, Phone, Mail, MapPin, Calendar, Image as ImageIcon } from "lucide-react";

// --- Utility: dark mode toggle persists to localStorage ---
function useDarkMode() {
  const [enabled, setEnabled] = useState(false);
  useEffect(() => {
    const saved = localStorage.getItem("prefers-dark") === "true";
    setEnabled(saved);
  }, []);
  useEffect(() => {
    const root = document.documentElement;
    if (enabled) root.classList.add("dark");
    else root.classList.remove("dark");
    localStorage.setItem("prefers-dark", String(enabled));
  }, [enabled]);
  return { enabled, setEnabled } as const;
}

// --- Components ---
const Container: React.FC<{ className?: string; id?: string }> = ({ className = "", id, children }) => (
  <div id={id} className={`mx-auto w-full max-w-6xl px-4 sm:px-6 lg:px-8 ${className}`}>{children}</div>
);

const NavLink: React.FC<{ href: string; label: string; onClick?: () => void }> = ({ href, label, onClick }) => (
  <a
    href={href}
    onClick={onClick}
    className="rounded-xl px-3 py-2 text-sm font-medium text-gray-700/90 hover:bg-gray-100 hover:text-gray-900 dark:text-gray-300 dark:hover:bg-white/5 dark:hover:text-white"
  >
    {label}
  </a>
);

const SectionTitle: React.FC<{ title: string; subtitle?: string }> = ({ title, subtitle }) => (
  <div className="mb-10 text-center">
    <h2 className="text-3xl font-bold tracking-tight text-gray-900 dark:text-white sm:text-4xl">{title}</h2>
    {subtitle && (
      <p className="mt-3 text-base text-gray-600 dark:text-gray-300">{subtitle}</p>
    )}
  </div>
);

const FeatureCard: React.FC<{ icon: React.ReactNode; title: string; desc: string }> = ({ icon, title, desc }) => (
  <div className="rounded-2xl border border-gray-200 bg-white p-6 shadow-sm transition hover:shadow-md dark:border-white/10 dark:bg-gray-900/60">
    <div className="mb-4 inline-flex items-center justify-center rounded-2xl border border-gray-200 p-3 dark:border-white/10">
      {icon}
    </div>
    <h3 className="text-lg font-semibold text-gray-900 dark:text-white">{title}</h3>
    <p className="mt-2 text-sm leading-6 text-gray-600 dark:text-gray-300">{desc}</p>
  </div>
);

const GalleryItem: React.FC<{ src: string; alt: string }> = ({ src, alt }) => (
  <figure className="group overflow-hidden rounded-2xl border border-gray-200 bg-gray-50 dark:border-white/10 dark:bg-gray-900">
    <img src={src} alt={alt} className="h-56 w-full object-cover transition group-hover:scale-105" />
    <figcaption className="p-3 text-xs text-gray-600 dark:text-gray-300">{alt}</figcaption>
  </figure>
);

// --- Page ---
export default function Homepage() {
  const [mobileOpen, setMobileOpen] = useState(false);
  const { enabled, setEnabled } = useDarkMode();

  useEffect(() => {
    const onHashLink = (e: Event) => {
      const target = e.target as HTMLAnchorElement;
      if (target.tagName === "A" && target.hash) {
        setMobileOpen(false);
      }
    };
    document.addEventListener("click", onHashLink);
    return () => document.removeEventListener("click", onHashLink);
  }, []);

  return (
    <div className="min-h-screen bg-white text-gray-900 antialiased dark:bg-gray-950 dark:text-white">
      {/* Header / Navbar */}
      <header className="sticky top-0 z-50 border-b border-gray-200 bg-white/80 backdrop-blur supports-[backdrop-filter]:bg-white/60 dark:border-white/10 dark:bg-gray-950/70">
        <Container className="flex items-center justify-between py-3">
          <a href="#home" className="flex items-center gap-2 font-semibold">
            <span className="inline-block h-8 w-8 rounded-2xl bg-gray-900/90 dark:bg-white" />
            <span className="text-base">브랜드/사이트명</span>
          </a>

          <nav className="hidden items-center gap-1 sm:flex">
            <NavLink href="#about" label="소개" />
            <NavLink href="#services" label="서비스" />
            <NavLink href="#gallery" label="갤러리" />
            <NavLink href="#schedule" label="일정" />
            <NavLink href="#contact" label="문의" />
          </nav>

          <div className="flex items-center gap-2">
            <button
              aria-label="테마 전환"
              onClick={() => setEnabled(!enabled)}
              className="inline-flex items-center justify-center rounded-xl border border-gray-200 p-2 transition hover:bg-gray-100 dark:border-white/10 dark:hover:bg-white/5"
            >
              {enabled ? <Sun className="h-5 w-5" /> : <Moon className="h-5 w-5" />}
            </button>
            <button
              className="inline-flex items-center justify-center rounded-xl border border-gray-200 p-2 sm:hidden dark:border-white/10"
              onClick={() => setMobileOpen(!mobileOpen)}
              aria-label="메뉴 토글"
            >
              {mobileOpen ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
            </button>
          </div>
        </Container>
        {/* Mobile menu */}
        {mobileOpen && (
          <div className="border-t border-gray-200 bg-white dark:border-white/10 dark:bg-gray-950 sm:hidden">
            <Container className="flex flex-col py-2">
              {[
                ["#about", "소개"],
                ["#services", "서비스"],
                ["#gallery", "갤러리"],
                ["#schedule", "일정"],
                ["#contact", "문의"],
              ].map(([href, label]) => (
                <NavLink key={href} href={href} label={label} onClick={() => setMobileOpen(false)} />
              ))}
            </Container>
          </div>
        )}
      </header>

      {/* Hero */}
      <section id="home" className="relative overflow-hidden">
        <div className="pointer-events-none absolute inset-0 bg-[radial-gradient(50%_50%_at_50%_0%,rgba(0,0,0,0.06),transparent_60%)] dark:bg-[radial-gradient(50%_50%_at_50%_0%,rgba(255,255,255,0.08),transparent_55%)]" />
        <Container className="relative py-20 sm:py-28">
          <motion.h1
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.6 }}
            className="text-balance text-4xl font-extrabold tracking-tight sm:text-6xl"
          >
            간결하고 우아한 <span className="underline decoration-4 underline-offset-8">원페이지 홈페이지</span>
          </motion.h1>
          <motion.p
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.8, delay: 0.1 }}
            className="mt-6 max-w-2xl text-lg leading-8 text-gray-600 dark:text-gray-300"
          >
            이 템플릿은 소개/서비스/갤러리/일정/문의 섹션으로 구성되어 있으며, Tailwind CSS와 React로 제작되었습니다. 텍스트와 이미지만 바꾸면 바로 배포할 수 있습니다.
          </motion.p>
          <div className="mt-8 flex flex-wrap gap-3">
            <a
              href="#contact"
              className="inline-flex items-center gap-2 rounded-2xl bg-gray-900 px-5 py-3 font-medium text-white shadow hover:translate-y-px hover:shadow-md active:translate-y-0 dark:bg-white dark:text-gray-900"
            >
              지금 문의하기 <ArrowRight className="h-5 w-5" />
            </a>
            <a
              href="#about"
              className="inline-flex items-center gap-2 rounded-2xl border border-gray-300 px-5 py-3 font-medium hover:bg-gray-50 dark:border-white/20 dark:hover:bg-white/5"
            >
              더 알아보기
            </a>
          </div>
        </Container>
      </section>

      {/* About */}
      <section id="about" className="border-t border-gray-200 py-16 dark:border-white/10 sm:py-24">
        <Container>
          <SectionTitle title="소개" subtitle="간단한 브랜드/기관 소개 문구를 여기에 입력하세요." />
          <div className="grid gap-8 md:grid-cols-2">
            <div className="rounded-2xl border border-gray-200 bg-white p-6 dark:border-white/10 dark:bg-gray-900/60">
              <h3 className="mb-2 text-xl font-semibold">우리의 미션</h3>
              <p className="text-gray-600 dark:text-gray-300">
                고객의 문제를 정확히 이해하고, 실용적이고 아름다운 해결책을 제공합니다. 이 문단은 자유롭게 수정하세요.
              </p>
            </div>
            <div className="rounded-2xl border border-gray-200 bg-white p-6 dark:border-white/10 dark:bg-gray-900/60">
              <h3 className="mb-2 text-xl font-semibold">핵심 가치</h3>
              <ul className="list-disc space-y-2 pl-5 text-gray-600 dark:text-gray-300">
                <li>정직과 신뢰</li>
                <li>지속 가능한 성장</li>
                <li>고객 중심의 설계</li>
              </ul>
            </div>
          </div>
        </Container>
      </section>

      {/* Services */}
      <section id="services" className="border-t border-gray-200 py-16 dark:border-white/10 sm:py-24">
        <Container>
          <SectionTitle title="서비스" subtitle="제공하는 서비스/상품을 간단히 정리하세요." />
          <div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
            <FeatureCard icon={<Phone className="h-5 w-5" />} title="상담" desc="전화/메신저를 통한 빠른 상담." />
            <FeatureCard icon={<Calendar className="h-5 w-5" />} title="예약" desc="방문 시간 예약 및 관리." />
            <FeatureCard icon={<ImageIcon className="h-5 w-5" />} title="콘텐츠" desc="이미지/영상 자료 제공." />
          </div>
        </Container>
      </section>

      {/* Gallery */}
      <section id="gallery" className="border-t border-gray-200 py-16 dark:border-white/10 sm:py-24">
        <Container>
          <SectionTitle title="갤러리" subtitle="이미지 6장을 예시로 배치했습니다. 파일 경로만 교체하세요." />
          <div className="grid gap-4 sm:grid-cols-2 lg:grid-cols-3">
            {[
              "/images/sample1.jpg",
              "/images/sample2.jpg",
              "/images/sample3.jpg",
              "/images/sample4.jpg",
              "/images/sample5.jpg",
              "/images/sample6.jpg",
            ].map((src, i) => (
              <GalleryItem key={src} src={src} alt={`갤러리 이미지 ${i + 1}`} />
            ))}
          </div>
        </Container>
      </section>

      {/* Schedule */}
      <section id="schedule" className="border-t border-gray-200 py-16 dark:border-white/10 sm:py-24">
        <Container>
          <SectionTitle title="일정" subtitle="주간 운영 시간 또는 주요 이벤트 일정을 표시합니다." />
          <div className="overflow-hidden rounded-2xl border border-gray-200 dark:border-white/10">
            <table className="w-full text-left text-sm">
              <thead className="bg-gray-50 text-gray-600 dark:bg-gray-900/60 dark:text-gray-300">
                <tr>
                  <th className="px-4 py-3">요일</th>
                  <th className="px-4 py-3">운영 시간</th>
                  <th className="px-4 py-3">비고</th>
                </tr>
              </thead>
              <tbody className="divide-y divide-gray-200 dark:divide-white/10">
                {[
                  ["월", "09:00 - 18:00", "점심 12:30-13:30"],
                  ["화", "09:00 - 18:00", ""],
                  ["수", "09:00 - 18:00", ""],
                  ["목", "09:00 - 18:00", ""],
                  ["금", "09:00 - 18:00", ""],
                  ["토", "10:00 - 14:00", "예약 우선"],
                  ["일/공휴일", "휴무", ""]
                ].map(([d, h, n]) => (
                  <tr key={d}>
                    <td className="px-4 py-3">{d}</td>
                    <td className="px-4 py-3">{h}</td>
                    <td className="px-4 py-3 text-gray-600 dark:text-gray-300">{n}</td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </Container>
      </section>

      {/* Contact */}
      <section id="contact" className="border-t border-gray-200 py-16 dark:border-white/10 sm:py-24">
        <Container>
          <SectionTitle title="문의" subtitle="연락처, 위치, 간단한 문의 폼을 제공합니다." />
          <div className="grid gap-8 md:grid-cols-2">
            <div className="rounded-2xl border border-gray-200 p-6 dark:border-white/10">
              <h3 className="mb-4 text-lg font-semibold">연락처</h3>
              <ul className="space-y-3 text-sm">
                <li className="flex items-center gap-3"><Phone className="h-5 w-5" /><a href="tel:010-0000-0000" className="hover:underline">010-0000-0000</a></li>
                <li className="flex items-center gap-3"><Mail className="h-5 w-5" /><a href="mailto:hello@example.com" className="hover:underline">hello@example.com</a></li>
                <li className="flex items-center gap-3"><MapPin className="h-5 w-5" />부산 사하구 ○○로 00 (예시)</li>
              </ul>
              <div className="mt-6 h-56 w-full overflow-hidden rounded-xl bg-gray-100 dark:bg-gray-900/60">
                {/* 지도를 삽입하려면 iframe 또는 이미지로 교체하세요 */}
                <div className="flex h-full items-center justify-center text-sm text-gray-500 dark:text-gray-400">지도 자리 (이미지/iframe)</div>
              </div>
            </div>

            <form
              className="rounded-2xl border border-gray-200 p-6 shadow-sm dark:border-white/10"
              onSubmit={(e) => {
                e.preventDefault();
                alert("폼이 제출된 것으로 가정합니다. 이메일 연동 또는 Google Form으로 교체하세요.");
              }}
            >
              <div className="mb-4">
                <label className="mb-1 block text-sm font-medium">이름</label>
                <input required className="w-full rounded-xl border border-gray-300 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-gray-900/20 dark:border-white/20 dark:bg-transparent" />
              </div>
              <div className="mb-4">
                <label className="mb-1 block text-sm font-medium">이메일</label>
                <input type="email" required className="w-full rounded-xl border border-gray-300 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-gray-900/20 dark:border-white/20 dark:bg-transparent" />
              </div>
              <div className="mb-4">
                <label className="mb-1 block text-sm font-medium">메시지</label>
                <textarea required rows={5} className="w-full rounded-xl border border-gray-300 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-gray-900/20 dark:border-white/20 dark:bg-transparent" />
              </div>
              <button type="submit" className="inline-flex items-center gap-2 rounded-2xl bg-gray-900 px-5 py-3 font-medium text-white shadow hover:translate-y-px hover:shadow-md active:translate-y-0 dark:bg-white dark:text-gray-900">
                보내기 <ArrowRight className="h-5 w-5" />
              </button>
            </form>
          </div>
        </Container>
      </section>

      {/* Footer */}
      <footer className="border-t border-gray-200 py-10 text-sm text-gray-600 dark:border-white/10 dark:text-gray-300">
        <Container className="flex flex-col items-center justify-between gap-4 sm:flex-row">
          <p>© {new Date().getFullYear()} 브랜드/사이트명. All rights reserved.</p>
          <div className="flex items-center gap-3">
            <a href="#about" className="hover:underline">소개</a>
            <a href="#services" className="hover:underline">서비스</a>
            <a href="#contact" className="hover:underline">문의</a>
          </div>
        </Container>
      </footer>
    </div>
  );
}

/*
사용 방법 (간단 요약)
1) 좌측 코드에서 텍스트(브랜드명, 연락처, 주소)와 이미지 경로(/images/*)만 바꾸면 됩니다.
2) GitHub Pages 배포 시: public/images 폴더에 이미지 파일을 넣고 경로를 맞추세요.
3) 폼 제출은 알림창으로 처리되어 있습니다. 실제 사용 시 Google Form 또는 Email API로 교체하세요.
*/
