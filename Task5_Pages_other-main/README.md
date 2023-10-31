# PAGES OTHER MAIN

# *Content*

1. [Cấu trúc](#1) 
   1. [Figma](#1.1)
   2. [Phân tích Layout](#1.2)
2. [Link](#2)
3. [Routes](#3)
4. [Layout](#4)
5. [Pages](#5)


# Cấu trúc <a id="1"></a>
> Những video trước trong khóa học chúng ta cùng nhau làm phần layout cho trang Home trong main trong lần này chung ta tiếp tục đi hoàn thiện các trang khác trong dự án, có thể xem file thiết kế `Figma`, thì trong main có tổ hợp trang `Other` có thiết kế tương đồng có thể hoàn thiện trực tiếp.
## Figma <a id="1.1"></a>
![Figma](./pages.png)
## Phân tích Layout <a id="1.2"></a>
> Layout chung của các trang này sẽ là:  
* cùng có path trực tiếp từ `/path` tức là ngang cấp với trang `home` và không có trang con ở bên trong.
* cùng có 1 dạng `layout` chỉ có `header` ==> `compact` không giống như của trang `Home`.

> Gồm các trang:
* **Error**:
  * `403`: *Lỗi này xảy ra khi bạn không được phép truy cập trang web. Điều này có thể do bạn không có quyền truy cập hoặc trang web đang được bảo vệ bằng mật khẩu.*
  * `404`:  *Lỗi này xảy ra khi trang web mà bạn đang cố truy cập không tồn tại. Điều này có thể do trang web đã bị xóa, di chuyển hoặc có lỗi.*
  * `500`: *Lỗi này xảy ra khi máy chủ web gặp sự cố khi xử lý yêu cầu của bạn. Điều này có thể do nhiều nguyên nhân, chẳng hạn như lỗi phần mềm, lỗi phần cứng hoặc quá tải.*
* **Other**:
  * `maintemance`: *được sử dụng để thông báo cho người dùng rằng trang web chính đang được bảo trì. Trang web này thường được tạo ra bởi người quản trị web khi họ cần thực hiện các thay đổi đối với trang web chính, chẳng hạn như cập nhật phần mềm, sửa lỗi hoặc thêm nội dung mới.*
  * `coming soon`: *một trang web tạm thời được sử dụng để thông báo cho người dùng rằng một trang web hoặc sản phẩm mới sắp được ra mắt. Trang web này thường được sử dụng bởi các doanh nghiệp, tổ chức hoặc cá nhân khi họ đang trong quá trình phát triển một trang web hoặc sản phẩm mới.*

> Để tạo ra những trang này cần chú ý các mục cần tạo:
* `Link`: tạo đường dẫn vào trang pages ở đây sẽ dẫn qua `list` của `navigation` của trang `Home`.
* `Routes`: tạo các đường path đến trang, phần này sẽ phát triển trong `mainRoutes` cùng trong mục `Routes` đang phát triển.
* `Layout`: Không thể dùng layout của main chúng ta tạo ra một layout riêng trong mục `compact` rất đơn giản chỉ gồm có header.
* `Pages`: Tạo ra nội dung `view` của từng trang web.

# Link <a id="2"></a>
> Như đã phân tích ở trên thì chung ta sẽ bắt đầu từ code của layout trang home cụ thể là trong phần `header` của `main/nav/header`  thêm đường dẫn vào `main/nav/config-navigation`:
```tsx
    export const navConfig = [
  {
    title: 'Home',
    icon: <Iconify icon="solar:home-2-bold-duotone" />,
    path: '/',
  },
  {
    title: 'Components',
    icon: <Iconify icon="solar:atom-bold-duotone" />,
    path: paths.components,
  },
  {
    title: 'Pages',
    path: '/pages',
    icon: <Iconify icon="solar:file-bold-duotone" />,
    children: [
        // tạo list trong pages vậy là có thế sử dụng được
      {
        subheader: 'Other',
        items: [
          { title: 'About us', path: paths.about },
          { title: 'Contact us', path: paths.contact },
          { title: 'Maintenance', path: paths.maintenance },
          { title: 'Coming Soon', path: paths.comingSoon },
        ],
      }, 
      {
        subheader: 'Error',
        items: [
          { title: 'Page 403', path: paths.page403 },
          { title: 'Page 404', path: paths.page404 },
          { title: 'Page 500', path: paths.page500 },
        ],
      },
      {
        subheader: 'Dashboard',
        items: [{ title: 'Dashboard', path: PATH_AFTER_LOGIN }],
      },
    ],
  },
  {
    title: 'Docs',
    icon: <Iconify icon="solar:notebook-bold-duotone" />,
    path: paths.docs,
  },
];
```
> có đường đẫn rồi nhưng phải cho thêm vào `paths` dẫn đường:
```ts
    const ROOTS = {
  DASHBOARD: '/dashboard',
};

export const paths = {
  components: 'components',
  docs: 'docs',
  about: '/about-us',
  contact: '/contact-us',
  faqs: '/faqs',
  //other
  comingSoon: '/coming-soon',
  maintenance: '/maintenance',
  //error
  page403: '/403',
  page404: '/404',
  page500: '/500',
  //external
  changelog: 'https://google.com',
  minimalUI: 'https://google.com',

  //dashboard
  dashboard: {
    root: ROOTS.DASHBOARD,
  },
};
```
> Đến đây là có các lựa chọn dẫn nhưng chưa có đường dẫn thực tế nào vào chưa tồn tại một pages hiển thị nào cả
# Routes <a id="3"></a>
> cần thêm các lựa chọn vào trong file mainRouter
```ts
    {
    element: (
        //tạo compact trong mục layout
      <CompactLayout>
        <Suspense fallback={<SplashScreen />}>
          <Outlet />
        </Suspense>
      </CompactLayout>
    ),
    children: [
        //tạo pages 
      { path: 'coming-soon', element: <ComingSoonPage /> },
      { path: 'maintenance', element: <MaintenancePage /> },
      { path: '500', element: <Page500 /> },
      { path: '404', element: <Page404 /> },
      { path: '403', element: <Page403 /> },
    ],
  },
```
# Layout <a id="4"></a>
> tạo `CompastLayout` trong layouts/compact/layout.tsx
```tsx
    // @mui
import Stack from '@mui/material/Stack';
import Container from '@mui/material/Container';
//
import { HeaderSimple as Header } from '../_common';

// ----------------------------------------------------------------------

type Props = {
  children: React.ReactNode;
};

export default function CompactLayout({ children }: Props) {
  return (
    <>
      <Header />

      <Container component="main">
        <Stack
          sx={{
            py: 12,
            m: 'auto',
            maxWidth: 400,
            minHeight: '100vh',
            textAlign: 'center',
            justifyContent: 'center',
          }}
        >
          {children}
        </Stack>
      </Container>
    </>
  );
}
```
> tạo `Header` trong thư mục `./_common/header-simple`
```tsx
    return (
    <AppBar>
      <Toolbar
        sx={{
          justifyContent: 'space-between',
          height: {
            xs: HEADER.H_MOBILE,
            md: HEADER.H_DESKTOP,
          },
          transition: theme.transitions.create(['height'], {
            easing: theme.transitions.easing.easeInOut,
            duration: theme.transitions.duration.shorter,
          }),
          ...(offsetTop && {
            ...bgBlur({
              color: theme.palette.background.default,
            }),
            height: {
              md: HEADER.H_DESKTOP_OFFSET,
            },
          }),
        }}
      >
        <Logo />

        <Stack direction="row" alignItems="center" spacing={1}>
          <SettingsButton />

          <Link
            href={paths.faqs}
            component={RouterLink}
            color="inherit"
            sx={{ typography: 'subtitle2' }}
          >
            Need help?
          </Link>
        </Stack>
      </Toolbar>

      {offsetTop && <HeaderShadow />}
    </AppBar>
  );
}
```

# Pages <a id="5"></a>

1. ***Error*** 
   1. *Page 403*
    ![403](403.png)
> Pages (src/pages/403.tsx)

```tsx
    import { Helmet } from 'react-helmet-async';
    // sections
    import { View403 } from 'src/sections/error';

    // ----------------------------------------------------------------------

    export default function Page403() {
    return (
        <>
        <Helmet>
            <title> 403 Forbidden</title>
        </Helmet>

        <View403 />
        </>
    );
    }
```
> 403-view (src/sections/error/...)
```tsx
  import { m } from 'framer-motion';
// @mui
import Button from '@mui/material/Button';
import Typography from '@mui/material/Typography';
// assets
import { ForbiddenIllustration } from 'src/assets/illustrations';
// components
import { RouterLink } from 'src/routes/components';
import { MotionContainer, varBounce } from 'src/components/animate';

// ----------------------------------------------------------------------

export default function View403() {
  return (
    //tạo animations
    <MotionContainer>
      <m.div variants={varBounce().in}>
        <Typography variant="h3" sx={{ mb: 2 }}>
          No permission
        </Typography>
      </m.div>

      <m.div variants={varBounce().in}>
        <Typography sx={{ color: 'text.secondary' }}>
          The page you&apos;re trying access has restricted access.
          <br />
          Please refer to your system administrator
        </Typography>
      </m.div>

      <m.div variants={varBounce().in}>
        <ForbiddenIllustration sx={{ height: 260, my: { xs: 5, sm: 10 } }} />
      </m.div>

      <Button component={RouterLink} href="/" size="large" variant="contained">
        Go to Home
      </Button>
    </MotionContainer>
  );
} 
```  

   2. *Page 404*: tuong tự như trong 403
    ![404](404.png)
   3. *Page 500*: tuong tự như trong 403
        ![500](500.png)

2.***Other***
   1. *Coming Soon*
   ![Coming Soon](comingSoon.png)
> Pages Coming Soon (./src/pages/comingSoon)

```tsx
        import { Helmet } from 'react-helmet-async';
        // sections
        import ComingSoonView from 'src/sections/coming-soon/view';

        // ----------------------------------------------------------------------

        export default function ComingSoonPage() {
        return (
            <>
            <Helmet>
                <title> Coming Soon</title>
            </Helmet>

            <ComingSoonView />
            </>
        );
        }
```

> Maintenance-view (./src/pages/Maintenance)
```tsx
    // @mui
import { alpha } from '@mui/material/styles';
import Box from '@mui/material/Box';
import Stack from '@mui/material/Stack';
import Button from '@mui/material/Button';
import TextField from '@mui/material/TextField';
import IconButton from '@mui/material/IconButton';
import Typography from '@mui/material/Typography';
import InputAdornment from '@mui/material/InputAdornment';
import { outlinedInputClasses } from '@mui/material/OutlinedInput';
// hooks
import { useCountdownDate } from 'src/hooks/use-countdown';
// _mock
import { _socials } from 'src/_mock';
// assets
import { ComingSoonIllustration } from 'src/assets/illustrations';
// components
import Iconify from 'src/components/iconify';

// ----------------------------------------------------------------------

export default function ComingSoonView() {
  const { days, hours, minutes, seconds } = useCountdownDate(new Date('07/07/2024 21:30'));

  return (
    <>
      <Typography variant="h3" sx={{ mb: 2 }}>
        Coming Soon!
      </Typography>

      <Typography sx={{ color: 'text.secondary' }}>
        We are currently working hard on this page!
      </Typography>

      <ComingSoonIllustration sx={{ my: 10, height: 240 }} />

      <Stack
        direction="row"
        justifyContent="center"
        divider={<Box sx={{ mx: { xs: 1, sm: 2.5 } }}>:</Box>}
        sx={{ typography: 'h2' }}
      >
        <TimeBlock label="Days" value={days} />

        <TimeBlock label="Hours" value={hours} />

        <TimeBlock label="Minutes" value={minutes} />

        <TimeBlock label="Seconds" value={seconds} />
      </Stack>

      <TextField
        fullWidth
        placeholder="Enter your email"
        InputProps={{
          endAdornment: (
            <InputAdornment position="end">
              <Button variant="contained" size="large">
                Notify Me
              </Button>
            </InputAdornment>
          ),
          sx: {
            pr: 0.5,
            [`&.${outlinedInputClasses.focused}`]: {
              boxShadow: (theme) => theme.customShadows.z20,
              transition: (theme) =>
                theme.transitions.create(['box-shadow'], {
                  duration: theme.transitions.duration.shorter,
                }),
              [`& .${outlinedInputClasses.notchedOutline}`]: {
                border: (theme) => `solid 1px ${alpha(theme.palette.grey[500], 0.32)}`,
              },
            },
          },
        }}
        sx={{ my: 5 }}
      />

      <Stack spacing={1} alignItems="center" justifyContent="center" direction="row">
        {_socials.map((social) => (
          <IconButton
            key={social.name}
            sx={{
              color: social.color,
              '&:hover': {
                bgcolor: alpha(social.color, 0.08),
              },
            }}
          >
            <Iconify icon={social.icon} />
          </IconButton>
        ))}
      </Stack>
    </>
  );
}

// ----------------------------------------------------------------------

type TimeBlockProps = {
  label: string;
  value: string;
};

function TimeBlock({ label, value }: TimeBlockProps) {
  return (
    <div>
      <Box> {value} </Box>
      <Box sx={{ color: 'text.secondary', typography: 'body1' }}>{label}</Box>
    </div>
  );
}

```
   1. *Maintenance*: Xây dựng tương tự như trên
      ![maintenance](maintenance.png) 
    
