<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Signature Generator</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<style>
:root{
  --navy:#132A45;--ink:#101828;--sub:#64748B;--line:#E2E8F0;
  --mint:#34D399;--mint-dark:#059669;--mint-pale:#ECFDF5;--mint-pale2:#D1FAE5;
  --cream:#FBF0D9;--amber:#B45309;
  --card:#FFFFFF;
}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Inter',sans-serif;background:#FFFFFF;color:var(--ink)}
.wrapper{max-width:1200px;margin:0 auto;padding:20px 24px 60px}

/* promo banner */
.promo{background:var(--mint-pale2);border-radius:14px;padding:14px 22px;text-align:center;font-size:14px;font-weight:600;color:#065F46;margin:14px 0 34px}
.promo a{color:#065F46;font-weight:800;text-decoration:underline;margin-left:6px}

/* gallery heading */
.gallery-head{display:flex;justify-content:space-between;align-items:flex-end;flex-wrap:wrap;gap:16px;margin-bottom:22px}
.gallery-head h2{font-size:30px;font-weight:800;color:var(--navy);letter-spacing:-.01em}
.gallery-head h2 em{font-style:italic;color:var(--mint-dark)}
.gallery-head p{color:var(--sub);font-size:14px;margin-top:4px}
.gallery-actions{display:flex;align-items:center;gap:12px}
#selectedLabel{font-size:11.5px;font-weight:800;letter-spacing:.06em;color:#94A3B8}
.btn-mint{background:var(--mint);color:#04321F;border:none;padding:11px 18px;border-radius:10px;font-weight:800;font-size:13.5px;cursor:pointer;font-family:inherit;display:inline-flex;align-items:center;gap:6px}
.btn-outline{background:#fff;border:1.5px solid var(--line);color:var(--ink);padding:10px 18px;border-radius:10px;font-weight:700;font-size:13.5px;cursor:pointer;font-family:inherit}

/* gallery grid */
.gallery-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:48px}
.style-card{border:1.5px solid var(--line);border-radius:16px;padding:18px;cursor:pointer;background:#fff;transition:border-color .15s,background .15s}
.style-card:hover{border-color:#B7E4D3}
.style-card.selected{border-color:var(--mint-dark);background:var(--mint-pale)}
.style-card-head{display:flex;align-items:flex-start;gap:10px;margin-bottom:14px;position:relative}
.radio-dot{width:18px;height:18px;border-radius:50%;border:2px solid var(--line);flex-shrink:0;margin-top:1px}
.style-card.selected .radio-dot{border-color:var(--mint-dark);background:radial-gradient(circle,var(--mint-dark) 40%,transparent 42%)}
.style-card-head h4{font-size:14.5px;font-weight:700}
.style-card-head p{font-size:12px;color:var(--sub);margin-top:1px}
.selected-badge{margin-left:auto;font-size:10px;font-weight:800;letter-spacing:.05em;color:var(--mint-dark);background:var(--mint-pale2);padding:4px 8px;border-radius:6px}
.style-preview{background:#F8FBFA;border:1px solid var(--line);border-radius:12px;padding:16px;overflow:auto}

/* builder */
.builder{margin-bottom:48px}
.builder-head{display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px;margin-bottom:28px}
.eyebrow{display:flex;align-items:center;gap:8px;font-size:12px;font-weight:800;letter-spacing:.08em;color:var(--mint-dark);text-transform:uppercase}
.eyebrow-dot{width:8px;height:8px;border-radius:50%;background:var(--mint)}
.builder-hint{font-size:12.5px;color:var(--sub)}

.section-block{margin-bottom:34px}
.section-title{display:flex;align-items:center;gap:12px;margin-bottom:20px}
.section-num{width:26px;height:26px;border-radius:8px;background:var(--mint-pale2);color:var(--mint-dark);font-weight:800;font-size:13px;display:flex;align-items:center;justify-content:center;flex-shrink:0}
.section-title h3{font-size:16px;font-weight:800}

.field-row{display:grid;gap:18px;margin-bottom:18px}
.cols4{grid-template-columns:repeat(4,1fr)}
.cols3{grid-template-columns:repeat(3,1fr)}
label{display:block;font-size:11px;font-weight:700;color:#475569;margin-bottom:7px;text-transform:uppercase;letter-spacing:.03em}
label .opt{font-weight:500;color:#94A3B8;text-transform:none}
input[type=text]{width:100%;padding:12px 13px;border:1.5px solid var(--line);border-radius:11px;background:#fff;font-size:14px;font-family:inherit;color:var(--ink)}
input[type=text]:focus{outline:none;border-color:var(--mint-dark)}
.field.invalid input{border-color:#DC2626}
.error{display:none;color:#DC2626;font-size:11.5px;margin-top:5px;font-weight:600}
.field.invalid .error{display:block}

.upload-box{border:1.5px dashed var(--line);border-radius:12px;padding:16px;cursor:pointer;display:flex;align-items:center;gap:12px;background:#FAFCFB}
.upload-box:hover{border-color:var(--mint-dark)}
.upload-icon{width:30px;height:30px;border-radius:8px;background:var(--mint-pale2);color:var(--mint-dark);display:flex;align-items:center;justify-content:center;font-size:16px;font-weight:800;flex-shrink:0}
.upload-text strong{display:block;font-size:13px}
.upload-text span{font-size:11.5px;color:var(--sub)}
.upload-box img{width:44px;height:44px;object-fit:cover;border-radius:8px;flex-shrink:0}
.upload-remove{margin-left:auto;font-size:11px;font-weight:700;color:#DC2626;background:none;border:none;cursor:pointer;font-family:inherit}

.pill-row{display:flex;gap:10px}
.shape-pill{border:1.5px solid var(--line);background:#fff;padding:10px 18px;border-radius:999px;font-size:13px;font-weight:700;cursor:pointer;font-family:inherit;color:var(--ink)}
.shape-pill.active{border-color:var(--mint-dark);background:var(--mint-pale);color:var(--mint-dark)}

.swatch-row{display:flex;gap:12px;align-items:center;flex-wrap:wrap;margin-top:4px}
.swatch{width:34px;height:34px;border-radius:50%;cursor:pointer;border:2px solid transparent;box-shadow:0 0 0 1px rgba(0,0,0,.06)}
.swatch.active{border-color:#fff;box-shadow:0 0 0 2px var(--navy)}
.custom-swatch-row{display:flex;align-items:center;gap:10px;margin-top:14px}
.custom-swatch{width:30px;height:30px;border-radius:50%;border:2px dashed var(--line);cursor:pointer;background:#fff}
.custom-swatch.active{border-style:solid;border-color:var(--navy)}
.custom-label{font-size:13px;font-weight:600;color:#475569;cursor:pointer}

/* CTA */
.cta-section{background:var(--mint-pale);border-radius:22px;padding:40px 44px}
.cta-badge{display:inline-block;background:var(--cream);color:var(--amber);font-size:11px;font-weight:800;letter-spacing:.08em;padding:7px 14px;border-radius:999px;margin-bottom:16px}
.cta-grid{display:flex;justify-content:space-between;align-items:center;gap:30px;flex-wrap:wrap}
.cta-grid h2{font-size:26px;font-weight:800;color:var(--navy);max-width:520px}
.cta-grid h2 em{font-style:italic;color:var(--mint-dark)}
.cta-grid p{color:#3F5B54;font-size:14px;margin-top:12px;max-width:520px;line-height:1.6}
.cta-buttons{display:flex;flex-direction:column;gap:10px;min-width:220px}
.cta-buttons button{padding:13px 20px;border-radius:12px;font-weight:800;font-size:14px;cursor:pointer;font-family:inherit}
.cta-buttons .fill{background:var(--mint);color:#04321F;border:none}
.cta-buttons .ghost{background:#fff;color:var(--navy);border:1.5px solid var(--line)}

@media(max-width:900px){.gallery-grid{grid-template-columns:1fr}.cols4,.cols3{grid-template-columns:1fr 1fr}.cta-grid{flex-direction:column;align-items:flex-start}}
@media(max-width:560px){.cols4,.cols3{grid-template-columns:1fr}}
</style>
</head>
<body>
<div class="wrapper">

  <div class="builder">
    <div class="builder-head">
      <div class="eyebrow"><span class="eyebrow-dot"></span>Signature Builder</div>
      <div class="builder-hint">Hit generate to refresh previews · nothing is stored</div>
    </div>

    <div class="section-block">
      <div class="section-title"><span class="section-num">1</span><h3>Your details</h3></div>
      <div class="field-row cols4">
        <div class="field" id="fullNameField">
          <label for="fullName">Full Name</label>
          <input id="fullName" type="text" placeholder="Ananya Sharma">
          <small class="error">Please enter a name.</small>
        </div>
        <div class="field">
          <label for="jobTitle">Job Title</label>
          <input id="jobTitle" type="text" placeholder="Marketing Lead">
        </div>
        <div class="field">
          <label for="company">Company</label>
          <input id="company" type="text" placeholder="Road to Top 5">
        </div>
        <div class="field">
          <label for="phone">Phone</label>
          <input id="phone" type="text" placeholder="+91 90358 34757">
        </div>
      </div>
      <div class="field-row cols3">
        <div class="field">
          <label for="email">Email</label>
          <input id="email" type="text" placeholder="you@company.com">
        </div>
        <div class="field">
          <label for="website">Website</label>
          <input id="website" type="text" placeholder="yourcompany.com">
        </div>
        <div class="field">
          <label for="linkedin">LinkedIn <span class="opt">optional</span></label>
          <input id="linkedin" type="text" placeholder="linkedin.com/in/you">
        </div>
      </div>
    </div>

    <div class="section-block">
      <div class="section-title"><span class="section-num">2</span><h3>Brand assets</h3></div>
      <div class="field-row cols3">
        <div class="field">
          <label>Company Logo</label>
          <div class="upload-box" id="logoBox">
            <div class="upload-icon">+</div>
            <div class="upload-text"><strong>Drop or click to upload</strong><span>PNG or SVG, transparent background</span></div>
          </div>
          <input type="file" id="logoInput" accept="image/*" hidden>
        </div>
        <div class="field">
          <label>Your Photo</label>
          <div class="upload-box" id="photoBox">
            <div class="upload-icon">+</div>
            <div class="upload-text"><strong>Drop or click to upload</strong><span>A square headshot crops cleanest</span></div>
          </div>
          <input type="file" id="photoInput" accept="image/*" hidden>
        </div>
        <div class="field">
          <label>Image Shape</label>
          <div class="pill-row" id="shapeRow">
            <button type="button" class="shape-pill active" data-shape="square">Square</button>
            <button type="button" class="shape-pill" data-shape="rounded">Rounded</button>
            <button type="button" class="shape-pill" data-shape="circle">Circle</button>
          </div>
        </div>
      </div>
      <label style="margin-top:6px">Accent Colour</label>
      <div class="swatch-row" id="swatchRow">
        <div class="swatch active" style="background:#34D399" data-color="#34D399"></div>
        <div class="swatch" style="background:#0F6B4A" data-color="#0F6B4A"></div>
        <div class="swatch" style="background:#F97316" data-color="#F97316"></div>
        <div class="swatch" style="background:#4F46E5" data-color="#4F46E5"></div>
        <div class="swatch" style="background:#EAB308" data-color="#EAB308"></div>
        <div class="swatch" style="background:#38BDF8" data-color="#38BDF8"></div>
        <div class="swatch" style="background:#DB2777" data-color="#DB2777"></div>
        <div class="swatch" style="background:#7C3AED" data-color="#7C3AED"></div>
      </div>
      <div class="custom-swatch-row">
        <div class="custom-swatch" id="customSwatch"></div>
        <span class="custom-label" id="customLabel">Custom</span>
        <input type="color" id="customColorInput" hidden value="#34D399">
      </div>
    </div>
  </div>

  <div class="gallery-head">
    <div>
      <h2>Ten signatures, <em>ready to use.</em></h2>
      <p>Pick one, then copy it straight into Gmail or Outlook.</p>
    </div>
    <div class="gallery-actions">
      <span id="selectedLabel">MODERN SELECTED</span>
      <button type="button" class="btn-mint" id="generateBtn">Generate signatures ↗</button>
      <button type="button" class="btn-outline" id="copyBtn">Copy signature HTML</button>
    </div>
  </div>

  <div class="gallery-grid" id="galleryGrid"></div>

  <div class="cta-section">
    <span class="cta-badge">LOOKING FOR MORE THAN A TOOL?</span>
    <div class="cta-grid">
      <div>
        <h2>Want us as your <em>SEO growth partner?</em></h2>
        <p>If the signature is the small detail, this is the big one. We run ROI-first SEO sprints for founders who want to rank on Google and get cited by AI answer engines — with weekly shipping and no long contracts.</p>
      </div>
      <div class="cta-buttons">
        <button type="button" class="fill">Hire us as your SEO partner</button>
        <button type="button" class="ghost">See our results</button>
      </div>
    </div>
  </div>

</div>

<script>
function byId(id){return document.getElementById(id)}
function val(id){return byId(id).value.trim()}

function normalizeUrl(u){
  u=(u||"").trim();
  if(!u) return "";
  if(/^mailto:|^tel:/i.test(u)) return u;
  if(!/^https?:\/\//i.test(u)) u="https://"+u;
  return u;
}
function initials(name){
  const parts=(name||"").trim().split(/\s+/).filter(Boolean);
  if(!parts.length) return "YN";
  return parts.slice(0,2).map(p=>p[0].toUpperCase()).join("");
}
function hexToRgb(hex){
  hex=(hex||"#34D399").replace('#','');
  if(hex.length===3) hex=hex.split('').map(c=>c+c).join('');
  const num=parseInt(hex,16);
  return {r:(num>>16)&255,g:(num>>8)&255,b:num&255};
}
function tint(hex,amount){
  const c=hexToRgb(hex);
  const r=Math.round(c.r+(255-c.r)*amount);
  const g=Math.round(c.g+(255-c.g)*amount);
  const b=Math.round(c.b+(255-c.b)*amount);
  return "rgb("+r+","+g+","+b+")";
}
function titleCompany(s){return [s.jobTitle,s.company].filter(Boolean).join(' · ')}
function contactItems(s){
  const items=[];
  if(s.phone) items.push({label:'P',value:s.phone,href:'tel:'+s.phone.replace(/[^0-9+]/g,'')});
  if(s.email) items.push({label:'E',value:s.email,href:'mailto:'+s.email});
  if(s.website) items.push({label:'W',value:s.website,href:normalizeUrl(s.website)});
  if(s.linkedin) items.push({label:'in',value:s.linkedin,href:normalizeUrl(s.linkedin)});
  return items;
}
function link(c){return '<a href="'+c.href+'" style="color:inherit;text-decoration:none">'+c.value+'</a>'}
function avatarBlock(s,size){
  size=size||56;
  const radius = s.imageShape==='circle'? '50%' : s.imageShape==='rounded' ? '14px':'0';
  const img=s.photoDataUrl||s.logoDataUrl;
  if(img) return '<img src="'+img+'" width="'+size+'" height="'+size+'" style="display:block;border-radius:'+radius+';object-fit:cover" alt="">';
  return '<table cellpadding="0" cellspacing="0" role="presentation"><tr><td width="'+size+'" height="'+size+'" align="center" valign="middle" style="background:'+s.accentColor+';border-radius:'+radius+';font-family:Arial,Helvetica,sans-serif;font-size:'+Math.round(size*0.36)+'px;font-weight:700;color:#ffffff;">'+initials(s.fullName)+'</td></tr></table>';
}

const STYLES={
  modern:{label:'Modern',desc:'Accent rule, name-forward',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(c=>'<div style="font-size:12px;color:#64748B;line-height:1.6">'+link(c)+'</div>').join('');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr>'+
      '<td width="56" style="padding-right:14px;vertical-align:top">'+avatarBlock(s,56)+'</td>'+
      '<td style="border-left:3px solid '+s.accentColor+';padding-left:14px;vertical-align:top">'+
      '<div style="font-size:17px;font-weight:700;color:#101828">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:13px;color:'+s.accentColor+';font-weight:600;margin-top:2px">'+tc+'</div>':'')+
      '<div style="margin-top:6px">'+contacts+'</div></td></tr></table>';
  }},
  minimal:{label:'Minimal',desc:'Plain text, nothing extra',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(link).join('&nbsp;&nbsp;&nbsp;');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr><td>'+
      '<div style="font-size:15px;font-weight:700;color:#101828">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:13px;color:#475569;margin-top:2px">'+tc+'</div>':'')+
      (contacts?'<div style="font-size:12px;color:#94A3B8;margin-top:4px">'+contacts+'</div>':'')+
      '</td></tr></table>';
  }},
  classic:{label:'Classic',desc:'Serif, labelled lines',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(c=>'<div style="font-size:12px;color:#475569;line-height:1.7"><span style="color:'+s.accentColor+';font-weight:700">'+c.label+':</span> '+link(c)+'</div>').join('');
    return '<table cellpadding="0" cellspacing="0" role="presentation"><tr><td>'+
      '<div style="font-family:Georgia,\'Times New Roman\',serif;font-size:18px;font-weight:700;color:#101828">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-family:Georgia,serif;font-style:italic;font-size:13px;color:#64748B;margin-top:2px">'+tc+'</div>':'')+
      '<div style="margin-top:8px;font-family:Arial,Helvetica,sans-serif">'+contacts+'</div></td></tr></table>';
  }},
  bold:{label:'Bold',desc:'Name in a colour block',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(link).join(' &middot; ');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr><td>'+
      '<table cellpadding="0" cellspacing="0" role="presentation"><tr><td style="background:'+s.accentColor+';border-radius:8px;padding:8px 14px"><span style="font-size:15px;font-weight:700;color:#ffffff">'+(s.fullName||'Your Name')+'</span></td></tr></table>'+
      (tc?'<div style="font-size:13px;color:#475569;margin-top:8px">'+tc+'</div>':'')+
      (contacts?'<div style="font-size:12px;color:#94A3B8;margin-top:4px">'+contacts+'</div>':'')+
      '</td></tr></table>';
  }},
  stacked:{label:'Stacked',desc:'Compact, single column',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(c=>'<div style="font-size:11px;color:#64748B;line-height:1.5">'+link(c)+'</div>').join('');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr><td>'+avatarBlock(s,40)+'</td></tr>'+
      '<tr><td style="padding-top:8px">'+
      '<div style="font-size:14px;font-weight:700;color:#101828;line-height:1.4">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:12px;color:#475569;line-height:1.4">'+tc+'</div>':'')+
      contacts+'</td></tr></table>';
  }},
  card:{label:'Card',desc:'Tinted panel with border',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(c=>'<div style="font-size:12px;color:#64748B;line-height:1.6">'+link(c)+'</div>').join('');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr><td style="background:'+tint(s.accentColor,0.9)+';border:1px solid '+s.accentColor+';border-radius:12px;padding:16px">'+
      '<table cellpadding="0" cellspacing="0" role="presentation"><tr>'+
      '<td width="52" style="padding-right:12px;vertical-align:top">'+avatarBlock(s,52)+'</td>'+
      '<td style="vertical-align:top"><div style="font-size:15px;font-weight:700;color:#101828">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:12.5px;color:#475569;margin-top:2px">'+tc+'</div>':'')+
      '<div style="margin-top:6px">'+contacts+'</div></td></tr></table></td></tr></table>';
  }},
  banner:{label:'Banner',desc:'Full-width colour strip on top',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(link).join('&nbsp;&nbsp;&middot;&nbsp;&nbsp;');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif;width:100%"><tr><td style="background:'+s.accentColor+';height:4px;font-size:0;line-height:0">&nbsp;</td></tr>'+
      '<tr><td style="padding-top:10px">'+
      '<div style="font-size:16px;font-weight:700;color:#101828">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:13px;color:#475569;margin-top:2px">'+tc+'</div>':'')+
      (contacts?'<div style="font-size:12px;color:#94A3B8;margin-top:6px">'+contacts+'</div>':'')+
      '</td></tr></table>';
  }},
  split:{label:'Split',desc:'Two columns, divider between',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(c=>'<div style="font-size:12px;color:#64748B;line-height:1.7">'+link(c)+'</div>').join('');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr>'+
      '<td style="padding-right:16px;vertical-align:top">'+
      '<div style="font-size:15px;font-weight:700;color:#101828">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:12.5px;color:'+s.accentColor+';font-weight:600;margin-top:2px">'+tc+'</div>':'')+
      '</td><td style="border-left:1px solid #E2E8F0;padding-left:16px;vertical-align:top">'+contacts+'</td></tr></table>';
  }},
  compact:{label:'Compact',desc:'One tight line of details',render(s){
    const parts=[s.fullName||'Your Name',titleCompany(s)].concat(contactItems(s).map(link)).filter(Boolean);
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr><td>'+
      '<span style="font-size:13px;color:#101828">'+parts.map((p,i)=> i===0? ('<strong>'+p+'</strong>') : p).join(' <span style="color:#CBD5E1">|</span> ')+'</span>'+
      '</td></tr></table>';
  }},
  underline:{label:'Underline',desc:'Name over a coloured rule',render(s){
    const tc=titleCompany(s);
    const contacts=contactItems(s).map(c=>'<div style="font-size:12px;color:#64748B;line-height:1.6">'+link(c)+'</div>').join('');
    return '<table cellpadding="0" cellspacing="0" role="presentation" style="font-family:Arial,Helvetica,sans-serif"><tr><td>'+
      '<div style="font-size:16px;font-weight:700;color:#101828;padding-bottom:5px;border-bottom:2px solid '+s.accentColor+';display:inline-block">'+(s.fullName||'Your Name')+'</div>'+
      (tc?'<div style="font-size:13px;color:#475569;margin-top:6px">'+tc+'</div>':'')+
      '<div style="margin-top:4px">'+contacts+'</div></td></tr></table>';
  }}
};
const STYLE_ORDER=['modern','minimal','classic','bold','stacked','card','banner','split','compact','underline'];

let selectedStyle='modern';
let draft={
  fullName:'Ananya Sharma',jobTitle:'Marketing Lead',company:'Road to Top 5',phone:'+91 90358 34757',
  email:'',website:'',linkedin:'',
  logoDataUrl:null,photoDataUrl:null,imageShape:'square',accentColor:'#34D399'
};
let state=Object.assign({},draft);

byId('fullName').value=draft.fullName;
byId('jobTitle').value=draft.jobTitle;
byId('company').value=draft.company;
byId('phone').value=draft.phone;

['fullName','jobTitle','company','phone','email','website','linkedin'].forEach(id=>{
  byId(id).addEventListener('input',()=>{
    draft[id]=byId(id).value.trim();
    if(id==='fullName') clearError('fullName');
  });
});

function markError(id){byId(id+'Field').classList.add('invalid');byId(id).focus()}
function clearError(id){byId(id+'Field').classList.remove('invalid')}

document.querySelectorAll('.shape-pill').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.shape-pill').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    draft.imageShape=btn.dataset.shape;
  });
});

document.querySelectorAll('#swatchRow .swatch').forEach(sw=>{
  sw.addEventListener('click',()=>{
    document.querySelectorAll('#swatchRow .swatch').forEach(s=>s.classList.remove('active'));
    byId('customSwatch').classList.remove('active');
    sw.classList.add('active');
    draft.accentColor=sw.dataset.color;
  });
});
byId('customSwatch').addEventListener('click',()=>byId('customColorInput').click());
byId('customColorInput').addEventListener('input',()=>{
  const c=byId('customColorInput').value;
  document.querySelectorAll('#swatchRow .swatch').forEach(s=>s.classList.remove('active'));
  const cs=byId('customSwatch');
  cs.classList.add('active');
  cs.style.background=c;
  cs.style.borderStyle='solid';
  draft.accentColor=c;
});

function wireUpload(boxId,inputId,key,defaultIconHtml,strong,sub){
  const box=byId(boxId),input=byId(inputId);
  box.addEventListener('click',()=>input.click());
  input.addEventListener('change',()=>{
    const file=input.files[0];
    if(!file) return;
    const reader=new FileReader();
    reader.onload=()=>{
      draft[key]=reader.result;
      box.innerHTML='<img src="'+reader.result+'" alt=""><div class="upload-text"><strong>'+file.name+'</strong><span>Click to replace</span></div><button type="button" class="upload-remove">Remove</button>';
      box.querySelector('.upload-remove').addEventListener('click',(e)=>{
        e.stopPropagation();
        draft[key]=null;
        box.innerHTML='<div class="upload-icon">+</div><div class="upload-text"><strong>'+strong+'</strong><span>'+sub+'</span></div>';
      });
    };
    reader.readAsDataURL(file);
  });
}
wireUpload('logoBox','logoInput','logoDataUrl','Drop or click to upload','Drop or click to upload','PNG or SVG, transparent background');
wireUpload('photoBox','photoInput','photoDataUrl','Drop or click to upload','Drop or click to upload','A square headshot crops cleanest');

function renderGallery(){
  const grid=byId('galleryGrid');
  grid.innerHTML='';
  STYLE_ORDER.forEach(key=>{
    const styleObj=STYLES[key];
    const card=document.createElement('div');
    card.className='style-card'+(key===selectedStyle?' selected':'');
    card.innerHTML=
      '<div class="style-card-head"><span class="radio-dot"></span><div><h4>'+styleObj.label+'</h4><p>'+styleObj.desc+'</p></div>'+
      (key===selectedStyle?'<span class="selected-badge">SELECTED</span>':'')+'</div>'+
      '<div class="style-preview">'+styleObj.render(state)+'</div>';
    card.addEventListener('click',()=>{selectedStyle=key;renderGallery();});
    grid.appendChild(card);
  });
  byId('selectedLabel').textContent=STYLES[selectedStyle].label.toUpperCase()+' SELECTED';
}

function generate(){
  if(!draft.fullName){markError('fullName');return}
  clearError('fullName');
  state=Object.assign({},draft);
  renderGallery();
}
byId('generateBtn').addEventListener('click',generate);

function copySignature(){
  const html=STYLES[selectedStyle].render(state);
  const btn=byId('copyBtn');
  const label=btn.textContent;
  const flash=(msg)=>{btn.textContent=msg;setTimeout(()=>btn.textContent=label,1500)};
  if(navigator.clipboard && window.isSecureContext && window.ClipboardItem){
    const blobHtml=new Blob([html],{type:'text/html'});
    const blobText=new Blob([html],{type:'text/plain'});
    navigator.clipboard.write([new ClipboardItem({'text/html':blobHtml,'text/plain':blobText})])
      .then(()=>flash('Copied!')).catch(()=>fallbackCopy(html,flash));
  }else{
    fallbackCopy(html,flash);
  }
}
function fallbackCopy(html,flash){
  try{
    const ta=document.createElement('textarea');
    ta.value=html;ta.style.position='fixed';ta.style.opacity='0';
    document.body.appendChild(ta);ta.select();
    document.execCommand('copy');
    document.body.removeChild(ta);
    flash('Copied!');
  }catch(e){flash('Copy failed')}
}
byId('copyBtn').addEventListener('click',copySignature);

renderGallery();
</script>
</body>
</html>
