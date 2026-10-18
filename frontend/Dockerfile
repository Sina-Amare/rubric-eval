FROM node:22-alpine

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

# The API URL is inlined into the client bundle at build time (Next.js bakes
# NEXT_PUBLIC_* during `next build`), so it must be set here, not just at runtime.
ARG NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
ENV NEXT_PUBLIC_API_BASE_URL=$NEXT_PUBLIC_API_BASE_URL
# next/font downloads Google Fonts here. Node gives each address only 250 ms to
# connect, which slow links (e.g. a VPN) exceed, so allow 2 s per attempt.
RUN NODE_OPTIONS=--network-family-autoselection-attempt-timeout=2000 npm run build

EXPOSE 3000
CMD ["npm", "run", "start"]
