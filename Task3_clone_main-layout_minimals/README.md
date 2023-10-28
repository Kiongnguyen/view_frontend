# **LAYOUT**
# ***MAIN***
# Header
> mục phức tạp nhất do có thanh navigition có tasbar và nhiều button liên quan đến các trang khác.
## header.tsx (src/main/header.tsx)
 **Định nghĩa một số hằng trong file**  
 ```tsx
 //sử dụng vào làm css về animation,bgBlur
  const theme = useTheme();
 //sử dụng để lấy ra giá trị boolean khi cuộn trang qua điểm H_DESKTOP
  const offsetTop = useOffSetTop(HEADER.H_DESKTOP);
 //chọn breakpoint ở gt md khi width > md ==> mdUp= true 
  const mdUp = useResponsive('up', 'md');
 ```

  1. *useOffSetTop:* (rsc/components/hooks/use-of-set-top)
   
   ```tsx
    //thư viện animations
    import { useScroll } from 'framer-motion';
    //hoocs react
    import { useState, useEffect, useMemo, useCallback } from 'react';
    //khai báo kiểu trên ts
    type ReturnType = boolean;
    interface UseScrollOptions extends Omit<ScrollOptions, 'container' | 'target'> {
    container?: React.RefObject<HTMLElement>;
    target?: React.RefObject<HTMLElement>;
    }
    //useOffSetTop
    export function useOffSetTop(top = 0, options?: UseScrollOptions): ReturnType {
    //scrollY: Lấy vị trí cuộn bằng useScroll.
    const { scrollY } = useScroll(options);
    //value: Một biến trạng thái để theo dõi xem khoảng cách đã được vượt qua hay chưa.
    const [value, setValue] = useState(false);
    //onOffSetTop: Một hàm gọi lại để xử lý các thay đổi vị trí cuộn.
    const onOffSetTop = useCallback(() => {
    scrollY.on('change', (scrollHeight) => {
      if (scrollHeight > top) {
        setValue(true);
      } else {
        setValue(false);
      }
    });
  }, [scrollY, top]);
    //useEffect: Thực thi onOffSetTop khi thành phần được gắn hoặc onOffSetTop thay đổi.
  useEffect(() => {
    onOffSetTop();
  }, [onOffSetTop]);
    //memoizedValue: Memoize trạng thái value để ngăn các lần hiển thị lại không cần thiết.
  const memoizedValue = useMemo(() => value, [value]);
    //return memoizedValue: Trả về giá trị đã ghi nhớ cho biết khoảng cách có bị vượt qua hay không.
  return memoizedValue;
}
   ```
2. *useResponsive:* (src\hooks\use-responsive.ts)
   
  ```tsx
    // @mui
    import { useTheme, Breakpoint } from '@mui/material/styles';
    import useMediaQuery from '@mui/material/useMediaQuery';

    // ----------------------------------------------------------------------

    type ReturnType = boolean;
    //query: Xác định loại media query cần đánh giá. Nó có thể là up, down, between, hoặc only.
    export type Query = 'up' | 'down' | 'between' | 'only';
    export type Value = Breakpoint | number;
    //start: Chỉ định điểm bắt đầu hoặc giá trị pixel cho media query.
    //end: Chỉ định điểm kết thúc hoặc giá trị pixel cho media query (chỉ áp dụng cho truy vấn between).
    export function useResponsive(query: Query, start?: Value, end?: Value): ReturnType {
    const theme = useTheme();

    const mediaUp = useMediaQuery(theme.breakpoints.up(start as Value));

    const mediaDown = useMediaQuery(theme.breakpoints.down(start as Value));

    const mediaBetween = useMediaQuery(theme.breakpoints.between(start as Value, end as Value));

    const mediaOnly = useMediaQuery(theme.breakpoints.only(start as Breakpoint));

    if (query === 'up') {
        return mediaUp;
    }

    if (query === 'down') {
        return mediaDown;
    }

    if (query === 'between') {
        return mediaBetween;
    }

    return mediaOnly;
    }

    // ----------------------------------------------------------------------

    type BreakpointOrNull = Breakpoint | null;
    //Hook useWidth xác định breakpoint hiện tại dựa trên các breakpoints của chủ đề Material UI. Nó trả về breakpoint hiện tại dưới dạng giá trị Breakpoint hoặc null nếu không có breakpoint nào khớp.
    export function useWidth() {
    const theme = useTheme();
    //đảo ngược thứ tự mảng breakpoint
    const keys = [...theme.breakpoints.keys].reverse();

    return (
        //Phương thức reduce được sử dụng để lặp qua các khóa breakpoint đảo ngược. Đối với mỗi khóa, nó kiểm tra xem viewport hiện tại có khớp với breakpoint hay không bằng cách sử dụng hàm useMediaQuery. Nếu tìm thấy breakpoint khớp, hàm reduce sẽ trả về breakpoint đó. Nếu không, nó tiếp tục lặp qua các khóa còn lại.
        keys.reduce((output: BreakpointOrNull, key: Breakpoint) => {
        const matches = useMediaQuery(theme.breakpoints.up(key));
        //Nếu không có breakpoint nào khớp, hàm reduce sẽ trả về null. Trong trường hợp này, biểu thức || 'xs' đảm bảo rằng breakpoint mặc định 'xs' sẽ được trả về thay vì null.
        return !output && matches ? key : output;
        }, null) || 'xs'
    );
    }

  ``` 

**Hiển thị header**
```tsx
    <Container sx={{ display: 'flex', alignItems: 'center' }}>
          <Badge>
            <Logo />
          </Badge>

          <Box sx={{ flexGrow: 1 }} />
            //reponsive thể hiện trên màn khích thước to hơn md ==> cho desktop
          {mdUp && <NavDesktop offsetTop={offsetTop} data={navConfig} />}
          <Stack alignItems="center" direction={{ xs: 'row', md: 'row-reverse' }}>
          <Button variant="contained" target="_blank" rel="noopener" href={paths.minimalUI}>
              Purchase Now
            </Button>

            {mdUp && <LoginButton />}
           
            <SettingsButton
              sx={{
                ml: { xs: 1, md: 0 },
                mr: { md: 2 },
              }}
            />
            //reponsive thể hiện trên màn khích thước nhỏ hơn md
            {!mdUp && <NavMobile offsetTop={offsetTop} data={navConfig} />}
          </Stack>
        </Container>
```

*Trong này có một số thẻ MUI:*  
* `Badge`: hiển thị thông tin trên logo hoặc một `button` (vd: số tin nhắn, thông tin ver)
* `Box`: giống thẻ `<div>`
* `Stack`: là thẻ `div` nhưng có thêm các thuộc tính `flex` có sẵn
* `Button`: là thẻ `<button>`
* `Container` : là thẻ bao chọn thường dùng ngoài cùng có paddding sẵn  
  
*Trong này có một số thẻ tự tạo components:*  
* `Logo`: hiển thị logo 
* `NavDesktop`: hiển thị navigtion trên dekstop
* `LoginButton`: nút Login
* `NavMobile`: hiển thị navigtion trên mobile  
  
## Navigation (src/layouts/main/nav)
   > Gồm: `desktop`, `mobile`, `types.ts`
### Types
> hiển thị types cấu hình của tất cả các file trong thư mục
```ts
    //@mui
    import { ListItemButtonProps } from "@mui/material/ListItemButton";

        export type NavItemProps = {
        title: string;
        path: string;
        icon?: React.ReactElement;
        children?: {
            subheader: string;
            items: {
            title: string;
            path: string;
            }[];
        }[];
        };

        export interface NavItemDesktopProps extends ListItemButtonProps {
        item: NavItemProps;
        offsetTop?: boolean;
        active?: boolean;
        open?: boolean;
        subItem?: boolean;
        externalLink?: boolean;
        }
        export interface NavItemMobileProps extends ListItemButtonProps {
        item: NavItemProps;
        active?: boolean;
        open?: boolean;
        externalLink?: boolean;
        }

        export type NavProps = {
        offsetTop: boolean;
        data: NavItemProps[];
        };
```
### mobile
  > hiển thị dao diện trên mobile, gồm có file: `nav-moile`, `nav-list`, `nav-item`.
#### nav-mobile (src/layouts/main/nav/mobile/nav-mobile)
> tạo ra dao diện thẻ NavMobile dùng trong header
```tsx
//data lấy từ file config là một prop nhập từ thẻ 
  export default function NavMobile({ offsetTop, data }: NavProps) {
    //lấy ra đường dẫn hiện tại 
  const pathname = usePathname();
    // định dạng giá trị boolean
  const nav = useBoolean();
    //dùng effect để định dạng giá trị cho nav khi mà truy cập đường link mới 
  useEffect(() => {
    if (nav.value) {
      nav.onFalse();
    }
  
  }, [pathname]);

  return (
    <>
    // nút menu thu gọn vì ở chế độ mobile
      <IconButton
        onClick={nav.onTrue}
        sx={{
          ml: 1,
          ...(offsetTop && {
            color: 'text.primary',
          }),
        }}
      >
        <SvgColor src="/assets/icons/navbar/ic_menu_item.svg" />
      </IconButton>
    //mở tasbar bên cạnh bên sau khi click vào icon menu
      <Drawer
        open={nav.value}
        onClose={nav.onFalse}
        PaperProps={{
          sx: {
            pb: 5,
            width: 260,
          },
        }}
      >
      // tạo một components mới để sử dụng scrollbar sư dụng thư viện ngoài simplebar-react
        <Scrollbar>
          <Logo sx={{ mx: 2.5, my: 3 }} />
          //hiển thị list menu
          <List component="nav" disablePadding>
            {data.map((link) => (
              <NavList key={link.title} item={link} />
            ))}
          </List>
        </Scrollbar>
      </Drawer>
    </>
  );
}
```

#### nav-list (src/layouts/main/nav/mobile/nav-list)
> tạo ra list menu dạng dọc của mobile
```tsx
  export default function NavList({ item }: NavListProps) {
  const pathname = usePathname();
  const { path, children } = item;
  // tạo đường truy cập trực tuyến 
  const externalLink = path.includes('http');
  const nav = useBoolean();

  return (
    <>
    //mỗi navItem là một item trong list có thể thể hiện dạng đóng mở  
      <NavItem
        item={item}
        open={nav.value}
        onClick={nav.onToggle}
        active={pathname === path}
        externalLink={externalLink}
      />

      {!!children && (
        // mở lits nhỏ trong item khi từ file config có children
        <Collapse in={nav.value} unmountOnExit>
        //một cái components để làm list
          <NavSectionVertical
            data={children}
            sx={{
              [`& .${listClasses.root}`]: {
                '&:last-of-type': {
                  [`& .${listItemButtonClasses.root}`]: {
                    height: 160,
                    backgroundSize: 'cover',
                    backgroundPosition: 'center',
                    bgcolor: 'background.neutral',
                    backgroundRepeat: 'no-repeat',
                    backgroundImage: 'url(/assets/illustrations/illustration_dashboard.png)',
                    [`& .${listItemTextClasses.root}`]: {
                      display: 'none',
                    },
                  },
                },
              },
            }}
          />
        </Collapse>
      )}
    </>
  );
}

```
#### nav-item (src/layouts/main/nav/mobile/nav-item)
> dùng để render các item
```tsx
  export default function NavItem({
  item,
  open,
  active,
  externalLink,
  ...other
}: NavItemMobileProps) {
  const { title, path, icon, children } = item;

  const renderContent = (
    //css từ file styles của item
    <ListItem active={active} {...other}>
      <ListItemIcon> {icon} </ListItemIcon>

      <ListItemText disableTypography primary={title} />
      //nếu có list con bên trong thì có icon arow hướng xuống 
      {!!children && (
        <Iconify
          width={16}
          icon={open ? 'eva:arrow-ios-downward-fill' : 'eva:arrow-ios-forward-fill'}
          sx={{ ml: 1 }}
        />
      )}
    </ListItem>
  );

  // External link
  if (externalLink) {
    return (
      <Link href={path} target="_blank" rel="noopener" underline="none">
        {renderContent}
      </Link>
    );
  }

  // Has child
  if (children) {
    return renderContent;
  }

  // Default
  return (
    <Link component={RouterLink} href={path} underline="none">
      {renderContent}
    </Link>
  );
}

```
#### style (src/layouts/main/nav/mobile/style)
> style lại cho file nav-item
```tsx
  type ListItemProps = Omit<NavItemMobileProps, 'item'>;

export const ListItem = styled(ListItemButton, {
  shouldForwardProp: (prop) => prop !== 'active',
})<ListItemProps>(({ active, theme }) => ({
  ...theme.typography.body2,
  color: theme.palette.text.secondary,
  height: 48,
  // Active
  ...(active && {
    color: theme.palette.primary.main,
    ...theme.typography.subtitle2,
    backgroundColor: alpha(theme.palette.primary.main, theme.palette.action.selectedOpacity),
  }),
}));

```
### Desktop
> có cấu trúc file gần giống của mobile nhưng mục đich dùng cho màn hình lớn đều gồm các file như mobile, gồm có file: `nav-desktop`, `nav-list`, `nav-item`.
#### nav-desktop (src/layouts/main/nav/mobile/nav-desktop)
>render hết ra trên thanh tasbar theo chiều ngang
```tsx
export default function NavDesktop({ offsetTop, data }: NavProps) {
  return (
    <Stack component="nav" direction="row" spacing={5} sx={{ mr: 2.5, height: 1 }}>
      {data.map((link) => (
        <NavList key={link.title} item={link} offsetTop={offsetTop} />
      ))}
    </Stack>
  );
}

```
#### nav-list (src/layouts/main/nav/desktop/nav-list)
> ở desktop thay NavSectionVertical bằng thẻ Portal có trong Mui
```tsx
  <NavItem
        item={item}
        offsetTop={offsetTop}
        active={active}
        open={nav.value}
        externalLink={externalLink}
        onMouseEnter={handleOpenMenu}
        onMouseLeave={nav.onFalse}
      />

      {!!children && nav.value && (
        <Portal>
          <Fade in={nav.value}>
          //css dạng paper được làm lại trong file style
            <StyledMenu
              onMouseEnter={handleOpenMenu}
              onMouseLeave={nav.onFalse}
              sx={{ display: 'flex' }}
            >
              {children.map((list) => (
                //mở ra những list nhỏ hơn trong item 
                <NavSubList
                  key={list.subheader}
                  subheader={list.subheader}
                  items={list.items}
                  isDashboard={list.subheader === 'Dashboard'}
                  onClose={nav.onFalse}
                />
              ))}
            </StyledMenu>
          </Fade>
        </Portal>
      )}
    </>
  );
}

// ----------------------------------------------------------------------
// tạo sublist
type NavSubListProps = {
  isDashboard: boolean;
  subheader: string;
  items: NavItemProps[];
  onClose: VoidFunction;
};

function NavSubList({ items, isDashboard, subheader, onClose }: NavSubListProps) {
  const pathname = usePathname();

  return (
    <Stack
      spacing={2}
      flexGrow={1}
      alignItems="flex-start"
      sx={{
        pb: 2,
        ...(isDashboard && {
          pb: 0,
          maxWidth: { md: 1 / 3, lg: 540 },
        }),
      }}
    >
      <StyledSubheader disableSticky>{subheader}</StyledSubheader>

      {items.map((item) =>
        isDashboard ? (
          <NavItemDashboard key={item.title} item={item} onClick={onClose} />
        ) : (
          <NavItem
            subItem
            key={item.title}
            item={item}
            active={pathname === item.path}
            onClick={onClose}
          />
        )
      )}
    </Stack>
  );
}

```
#### nav-item (src/layouts/main/nav/desktop/nav-item)
```tsx
  export const NavItem = forwardRef<HTMLDivElement, NavItemDesktopProps>(
  ({ item, open, offsetTop, active, subItem, externalLink, ...other }, ref) => {
    const { title, path, children } = item;
    //tương dự dạng mobile
    const renderContent = (
      <ListItem
        ref={ref}
        disableRipple
        offsetTop={offsetTop}
        subItem={subItem}
        active={active}
        open={open}
        {...other}
      >
        {title}

        {!!children && <Iconify width={16} icon="eva:arrow-ios-downward-fill" sx={{ ml: 1 }} />}
      </ListItem>
    );

    // External link
    if (externalLink) {
      return (
        <Link href={path} target="_blank" rel="noopener" underline="none">
          {renderContent}
        </Link>
      );
    }

    // Has child
    if (children) {
      return renderContent;
    }

    // Default
    return (
      <Link component={RouterLink} href={path} underline="none">
        {renderContent}
      </Link>
    );
  }
);

// ----------------------------------------------------------------------
// ở dạng trang dashboard 
interface NavItemDashboardProps extends LinkProps {
  item: NavItemProps;
}

export function NavItemDashboard({ item, sx, ...other }: NavItemDashboardProps) {
  return (
    <Link component={RouterLink} href={item.path} sx={{ width: 1, height: 1 }} {...other}>
      <CardActionArea
        sx={{
          height: 1,
          minHeight: 320,
          borderRadius: 1.5,
          color: 'text.disabled',
          bgcolor: 'background.neutral',
          px: { md: 3, lg: 10 },
          ...sx,
        }}
      >
        <m.div
          whileTap="tap"
          whileHover="hover"
          variants={{
            hover: { scale: 1.02 },
            tap: { scale: 0.98 },
          }}
        >
          <Box
            component="img"
            alt="illustration_dashboard"
            src="/assets/illustrations/illustration_dashboard.png"
          />
        </m.div>
      </CardActionArea>
    </Link>
  );
}

```

# ***FOOTER***
> gồm có 2 dao diện gồm khi ở trang home và không ở trang home 
```tsx
  export default function Footer() {
  const pathname = usePathname();

  const isHome = pathname === '/';
//dao diện ở trang home
  const simpleFooter = (
    <Box
      component="footer"
      sx={{
        py: 5,
        textAlign: 'center',
        position: 'relative',
        bgcolor: 'background.default',
      }}
    >
      <Container>
        <Logo sx={{ mb: 1, mx: 'auto' }} />

        <Typography variant="caption" component="div">
          © All rights reserved
          <br /> made by
          <Link href="https://minimals.cc/"> minimals.cc </Link>
        </Typography>
      </Container>
    </Box>
  );
//dao diện đây đủ không ở trang home
  const mainFooter = (
    <Box
      component="footer"
      sx={{
        position: 'relative',
        bgcolor: 'background.default',
      }}
    >
      <Divider /> //gạch ngang cách đoạn trong trang 

      <Container
        sx={{
          pt: 10,
          pb: 5,
          textAlign: { xs: 'center', md: 'unset' },
        }}
      >
        <Logo sx={{ mb: 3 }} />

        <Grid
          container
          justifyContent={{
            xs: 'center',
            md: 'space-between',
          }}
        >
          <Grid xs={8} md={3}>
            <Typography
              variant="body2"
              sx={{
                maxWidth: 270,
                mx: { xs: 'auto', md: 'unset' },
              }}
            >
              The starting point for your next project with Minimal UI Kit, built on the newest
              version of Material-UI ©, ready to be customized to your style.
            </Typography>
            //liên kết mạng xã hội
            <Stack
              direction="row"
              justifyContent={{ xs: 'center', md: 'flex-start' }}
              sx={{
                mt: 3,
                mb: { xs: 5, md: 0 },
              }}
            >
              {_socials.map((social) => (
                <IconButton
                  key={social.name}
                  sx={{
                    '&:hover': {
                      bgcolor: alpha(social.color, 0.08),
                    },
                  }}
                >
                  <Iconify color={social.color} icon={social.icon} />
                </IconButton>
              ))}
            </Stack>
          </Grid>
          // phần menu 
          <Grid xs={12} md={6}>
            <Stack spacing={5} direction={{ xs: 'column', md: 'row' }}>
              {LINKS.map((list) => (
                <Stack
                  key={list.headline}
                  spacing={2}
                  alignItems={{ xs: 'center', md: 'flex-start' }}
                  sx={{ width: 1 }}
                >
                  <Typography component="div" variant="overline">
                    {list.headline}
                  </Typography>

                  {list.children.map((link) => (
                    <Link
                      key={link.name}
                      component={RouterLink}
                      href={link.href}
                      color="inherit"
                      variant="body2"
                    >
                      {link.name}
                    </Link>
                  ))}
                </Stack>
              ))}
            </Stack>
          </Grid>
        </Grid>

        <Typography variant="body2" sx={{ mt: 10 }}>
          © 2021. All rights reserved
        </Typography>
      </Container>
    </Box>
  );

  return isHome ? simpleFooter : mainFooter;
}

```
> cơ bản sử dụng thẻ grid vá stack để reposive

# ***CONFIG-LAYOUT***
##  config-navigation.tsx (src/main/config-navigation.tsx)
> có một số hằng có sẵn cho việc xây dựng trang như mảng về link, icon, ...
```ts
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
    children: [...
```
## layout.tsx (src/main/layout.tsx)

> thiết lập cấu trúc trang ghép các phần header, footer, body vào thành một và xuất ra thẻ MainLayout để sử dụng bên ngoài.
```tsx
  <Box sx={{ display: 'flex', flexDirection: 'column', height: 1 }}>
  //header
      <Header />
  //body
      <Box
        component="main"
        sx={{
          flexGrow: 1,
          ...(!isHome && {
            pt: { xs: 8, md: 10 },
          }),
        }}
      >
        {children}
      </Box>
  //footer
      <Footer />
    </Box>
```